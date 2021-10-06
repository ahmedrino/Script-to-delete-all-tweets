# Script-to-delete-all-tweets
require 'twitter'

puts "starting"
# Initial setup (don't change)
one_week_ago = (Time.now - 60 * 60 * 24 * 7)
one_month_ago = (Time.now - 60 * 60 * 24 * 31)
three_motnhs_ago = (Time.now - 60 * 60 * 24 * 31 * 3)
six_months_ago = (Time.now - 60 * 60 * 24 * 31 * 6)
one_year_ago = (Time.now - 60 * 60 * 24 * 31 * 12)
two_years_ago = (Time.now - 60 * 60 * 24 * 31 * 12 * 2)

# Set your username here
twitter_username = "YourUsernameHere"

# Set this for the tweets you _don't_ want deleted
older_than = one_year_ago

# Enter your credentials here
client = Twitter::REST::Client.new do |config|
  config.consumer_key        = "-key-here-"
  config.consumer_secret     = "-key-here-"
  config.access_token        = "-key-here-"
  config.access_token_secret = "-key-here-"
end

puts "logging in"
#
#
#
# Once the above has been set, run this script with `ruby /path/to/file`
#
#
#
# --------------------------------------------- #

def collect_with_max_id(collection=[], max_id=nil, &block)
  response = yield(max_id)
  collection += response
  response.empty? ? collection.flatten : collect_with_max_id(collection, response.last.id - 1, &block)
end
puts "collect_with_max_id"

def client.get_all_tweets(user)
  collect_with_max_id do |max_id|
    options = {count: 200, include_rts: true}
    options[:max_id] = max_id unless max_id.nil?
    user_timeline(user, options)
  end
end
puts "Client.get_all_tweets"

all_tweets = client.get_all_tweets(twitter_username)
puts "get_all_tweets"
puts "all tweets #{all_tweets.size}"

tweets_to_delete = all_tweets.select do |tweet|
  tweet.created_at < older_than
end
puts tweets_to_delete.size
puts "tweets_to_delete"

puts "start destroy_status"
client.destroy_status(tweets_to_delete)
puts "end destroy_status"
https://www.allurneedhere.com/2021/10/delete-all-tweets.html
