

# Config.ru

Rack provides a minimal interface between Ruby frameworks and any web servers supporting Ruby. Informally, a Rack application is a thing that responds to `call` and takes a hash as an argument, returning an array of status as an integer, headers as a hash, and a body as an array with string elements.

Remember that a Rack application isn't a class, but a Ruby object. An instance of a class that you'll need to instantiate when you run it.

### NOTE

Every time you are told to run `rackup` on your commandline, make sure you have no other servers running. Otherwise, you'll get an error. This is because `rackup` is always going to try and run on the same port, and you can only have one thing running on any given port at a time.

## Simple Config.ru

1. Create a directory called `hello-rack`
2. `cd` into that directory and create a file called `config.ru`
3. At the top of the file, add `require 'rack'` (if you haven't done so already, on your command line type `gem install rack`)
4. Inside that file, create a class `HelloRack` that responds to a `call` method
5. This `call` method should accept one argument, `env`, and should return:
  * A status code of 200
  * A "Content-Type" header of "text/html"
  * A body of "Hello Rack!"

6. Your code should look something like this:

  ```ruby
  class HelloRack

    def call(env)
      [200, {"Content-Type" => "text/html"}, ["Hello Rack!"]]
    end

  end
  ```

7. At the bottom of the file, pass a new instance of your class to the `run` method to tell Rack to mount it when the file is run. `run` is a method provided by rack that you can only use if your app is run through `rackup` and not through pure Ruby.

  ```ruby
  run HelloRack.new
  ```

8. In your console, make sure you are in the `hello-rack` directory, and type `rackup` to mount your rack app.
9. Watch the output and visit the port listed (it's most likely 9292) at localhost. i.e,: [http://localhost:9292](http://localhost:9292)
10. You should see "Hello Rack!" in your browser. Nice job!
11. Notice that the `call` method takes an argument of `env`? Rack passes in information it receives about the request as a hash automatically. Try using `puts` to put that variable. Then restart the app, reload the browser, and look in your console. See what kind of information is stored in that hash. How would you access the information there? What kinds of things could you do with it? Write a conditional that accesses something stored in the hash, and changes the response from rack based on that. (Hint: `REQUEST_PATH`.)
12. Want to be fancy? Try to replace your entire config.ru (except for the `require 'rack'` line) with:

  ```ruby
  run lambda { |env| [200, {'Content-Type'=>'text/plain'}, ["Hello Rack!"]] }
  ```

  Why does that work?

13. Ok, now put it back to how it was before. That's enough fun for one day.

  *Note: You must restart your rack app everytime you make a change. Stop it with CTRL+C and start it again with `rackup`. We will never run a rack/sinatra app through the ruby command again, always rackup a `config.ru` file.*

## Some Separation

This is all well and good. We're now using a `config.ru` file properly to run our Rack app. But it kind of sucks to have all of our code in here, right? The purpose of this file, really, is to run our app. And that's it. So let's move some of this code into a better place.

1. Inside of the `hello-rack` directory, create another file, `hello_rack.rb`. Move the class definition of `HelloRack` from our config file into this new file. Then, at the top of `config.ru`, add this line:

```ruby
require_relative './hello_rack'
```

2. Now, try running `rackup` from your command line again, and visiting [http://localhost:9292](http://localhost:9292) in your browser again. It still works, sweet!

## Resources
* [Rack](http://rack.github.io/) - [Rack: a Ruby Webserver Interface](http://rack.github.io/)
* [Chris Blogs](http://chneukirchen.org/blog/) - [Introducing Rack](http://chneukirchen.org/blog/archive/2007/02/introducing-rack.html)
* [Pratik Naik's Blog](http://m.onkey.org/) - [Ruby on Rack #1 - Hello Rack!](http://m.onkey.org/ruby-on-rack-1-hello-rack)
* [Pratik Naik's Blog](http://m.onkey.org/) - [Ruby on Rack #2 - The Builder](http://m.onkey.org/ruby-on-rack-2-the-builder)