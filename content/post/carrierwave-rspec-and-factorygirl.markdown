+++
date = "2013-03-26T21:02:00-05:00"
title = "Carrierwave, RSpec and FactoryGirl"
categories = ["Carrierwave", "RSpec", "FactoryGirl", "Software Development", "Ruby"]
tags = ["Carrierwave", "RSpec", "FactoryGirl", "Software Development", "Ruby"]
slug = "2013/carrierwave-rspec-and-factorygirl"
+++
As part of a project I wanted to run some rspec tests on a model that had many children models using [Carrierwave](https://github.com/jnicklas/carrierwave) to upload files to S3.

My models look something like this:

```ruby
class User < ActiveRecord::Base
    ...
    has_many :images
    ...
end

class Image < ActiveRecord::Base
    ...
    belongs_to :user

    validates_presence_of :file
    ...
    mount_uploader :file, FileUploader
    ...
end
```
<!-- more -->

In order to this I needd a few things:

+ Rspec Tests
+ Store files locally (not on S3)
+ Clean up locally created files

# Creating the test object

To help create the test objects I use [FactoryGirl](https://github.com/thoughtbot/factory_girl).

```ruby
factory :user do
    ...
    after(:build) do |user, eval|
        user.images << FactoryGirl.build(:image, user: user)
    end
end

factory :image do
    user :user

    file File.open(File.join(Rails.root, '/spec/fixtures/files/image.png'))
end
```

In my user model spec I have the following:

```ruby
describe User do
    it { should have_many :images }

    before(:each){
        @user = FactoryGirl.create(:user)
    }

    describe "images" do

        it "should have multiple images" do

            @user.images.create({document_file:File.open(File.join(Rails.root, '/spec/fixtures/files/image.png'))})
            @user.images.create({document_file:File.open(File.join(Rails.root, '/spec/fixtures/files/image.png'))})


            @user.images.length.should eq(3)
        end
    end
end
```

# Store local for test

In production and development I didn't want Carrierwave to use S3 as the storage source. So in the `app/uploaders/file_uploader.rb` file I removed the line `storage :fog`. This is now being configured in a new `carrierwave.rb` initalizer:

```ruby
if Rails.env.test? or Rails.env.cucumber?
  CarrierWave.configure do |config|
    config.storage = :file
    config.enable_processing = false
  end
else
  CarrierWave.configure do |config|
    config.storage = :fog
  end
end
```

# Cleaning up files

```ruby
after(:each) do
    if Rails.env.test? || Rails.env.cucumber?
        @document.versions.each do |v|
            store_path = File.dirname(File.dirname(v.document_file.url))
            temp_path = v.document_file.cache_dir
            FileUtils.rm_rf(Dir["#{Rails.root}/public/#{store_path}/[^.]*"])
            FileUtils.rm_rf(Dir["#{temp_path}/[^.]*"])
        end
    end
end
```
