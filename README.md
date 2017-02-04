#Twitter Explorer

TwitterExplorer allows you to display the last 25 tweets for any given Twitter handle.

I live version for this app can be found [here](https://marcetuxexplorer.herokuapp.com/)

## Getting started

To run this web application locally you will need a locally installed version of Ruby 2.3.1, Rubygems, Bundler and Rails 5+, Postgresql and Redis

Follow these steps to run the app locally:

###1. Clone this repository
```
git clone https://github.com/hectorMiranda/twitterexplorer.git
```

###2. Install dependencies

Running the command below will genera a new Gemfile.lock
```
bundle install
```
Now create the database, postgresql is required.
```
rails db:create
```

###3. Install redis

```
wget http://download.redis.io/redis-stable.tar.gz
tar xvzf redis-stable.tar.gz
cd redis-stable
make
make install
```

###4. Set up environment variables for the Twitter API
API secrets are being read from environment variables
Add the following to your ~/.bashrc file

```
export REDIS_URL="redis://yourredisurl:18459"
export TWITTER_CONSUMER_KEY_API="24WGsVF..."
export TWITTER_API_SECRET="7k428br1x...."
export TWITTER_OAUTH_ACCESS_TOKEN="17267...."
export TWITTER_OAUTH_ACCESS_TOKEN_SECRET="AEIPtSI8zi...."
```

##Application Design


The TwitterClient class implements the caching logic and the handle of the REST requests. I choose the Twitter gem as it provides the boilerplate around interacting with the Twitter API.

The twitter gem raise Net::OpenTimeout if it can’t connect to the server and Net::ReadTimeout if reading the response from the server times out I simply handle both exceptions to return empty hashes, and set the timeout to 1 second. Instead of implementing this timeout-handling logic in both methods, I’m opting for a handle_timeouts function that takes a block. If the block raises an exception, handle_timeouts will catch the exception and return an empty hash.

I'm using Redis as my cache server, all of the caching logic is in handle_caching(options) and cache_key(options), the latter of which builds a unique key to store the cached response based on a given Twitter handle.

handle_caching(options) checks for the existence of a key and returns the payload if available. Otherwise, it yields to the block passed in and stores the result in Redis.


##Using the app
###Creating your first user

Go to: http://localhost:3000/users/sign_up


##Testing
