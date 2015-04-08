# TwitterRetweet
div class="container-fluid"
	h2 Welcome to the Nairuby TwitterBot

	p This is a simple web application created for <a href="https://twitter.com/nairubyke">@NairubyKE</a>, the goal of the application is to find tweets with a specific topic and retweet those.

	p The web application is built fully in Ruby.

	p It uses the Twitter Gem for integrating with the Twitter API. It uses Sinatra for its web application framework. The application uses Slim for HTML templating and Twitter Bootstrap for HTML, CSS and JS. Datamapper is used for database and deploys using Heroku.

	p For more indepth description on the different techniques used, see below.

	div class="panel panel-danger"
		div class="panel-heading"
			h4 TODO
		div class="panel-body"
			ul
				li Add PROCFILE for Heroku.
				li Improve error handling.
				li Add some tests.
				li Refactor code.
				li Enable editing of the database entries - for instance un-retweet tweets from the database.
				li Fix the static footer.
				li Implement a fixed top nav bar.
				li Ensure database deployment and continious integration.

	#twitter
	h3 Ruby Twitter gem
	p For integration to Twitter the <a href="http://www.rubydoc.info/gems/twitter">Ruby Twitter Gem</a> is being used. 

	h4 Installation
	pre gem install twitter
	
	h4 Configuration
	pre
		| 
			client = Twitter::REST::Client.new do |config|
  				config.consumer_key        = "YOUR_CONSUMER_KEY"
  				config.consumer_secret     = "YOUR_CONSUMER_SECRET"
  				config.access_token        = "YOUR_ACCESS_TOKEN"
  				config.access_token_secret = "YOUR_ACCESS_SECRET"
  			end

  	h4 Examples
  	h5 Searching for tweets
  	p This will search for all resents tweets and will take the first 100 results.
  	pre client.search(topic, result_type: "recent").take(100)

  	h5 Retweeting
  	p This will retweet the <i>id</i> 
  	pre client.retweet(id)

  	#sinatra
  	h3 Sinatra
  	p The application uses <a href="http://www.sinatrarb.com">Sinatra</a> as the web application framework. It enabled a quick and easy way to get the application running.

  	h4 Installation
  	pre gem install sinatra

  	h4 Examples
  	pre
  		|
  			# myapp.rb
  			require 'sinatra'

  			get '/' do
  				'Hello world!'
  			end

  	h4 Running the application
  	pre ruby myapp.rb

  	#slim
  	h3 Slim
  	p In conjunction with Sinatra, <a href="http://slim-lang.com">Slim</a> is used for html templating.

  	h4 Examples
  	pre
  		code
  			|
  				doctype html
				html
					head
						meta charset="utf-8"
						title Nairuby TwitterBot
						link rel="shortcut icon" type="image/x-icon" href="/img/favicon.ico"
						link rel="stylesheet" href="/css/bootstrap.min.css"
						link rel="stylesheet" href="/css/sticky-footer.css"
		
					body
						h1 Nairuby TwitterBot

						== yield

						script src="/js/bootstrap.min.js"
	p For more example visit the Slim hompepage.

  	#bootstrap
  	h3 Twitter Bootstrap
  	p For HTML, CSS and JS framework, the <a href="http://www.getbootstrap.com">Twitter Bootstrap</a> was chosen.

  	h4 Project setup
  	p For this application the simplest form for this was chosen, the bootstrap css files was direclty copied into the folders as statied below. The same was done for the js.

  	pre
  		code
  		|
  			project/
			├── public/
			│   ├── css/
			│   ├── js/
			│   ├── fonts/
			│   └── img/
			└── views/

	p The CSS and the JS was then imported, as seen in the Slim example previous.

	#datamapper
	h3 Datamapper
	p <a href="http://www.datamapper.org">Datamapper</a> is used for the database, it enabled fast and simple database setup and comes with several adapters for commonly used datastores. For this application the apatders used were; SQLite and Postgresql.

	p At the moment the database only contains one table, which is used for storing all retweets. It is used when searching for new tweets to retweet - if there already exists a tweet in the databse with the same ID as a tweet found it will not retweet that tweet. If the tweet is not in the databse - the application will add it to the database and retweet the tweet found.

	pre 
		code
			|
				class TweetsDB
					include DataMapper::Resource
					property :id, Serial
					property :tweet_id, String, :required => true
					property :tweet_user_screen_name, String
					property :tweet_text, String
					property :retweeted, Boolean
					property :retweeted_at, DateTime
				end 

	div class="panel panel-danger"
		div class="panel-heading"
			h4 TODO
		div class="panel-body"
			p Enable editing of the database entries - for instance un-retweet tweets from the database.

	#heroku
	h3 Heroku
	p For deployment <a href="http://www.heroku.com">Heroku</a> is being used. There is a great tutorial on how to get started on their webpage: <a href="https://devcenter.heroku.com/articles/getting-started-with-ruby">Getting Started with Runy on Heroku<a/>
