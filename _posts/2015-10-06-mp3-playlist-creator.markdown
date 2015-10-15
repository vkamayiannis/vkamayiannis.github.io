---
layout: post
title:  "An MP3 playlist creator"
date:   2015-10-06 17:10:00
categories: category
comments: true
---
After a couple of weeks developing simple applications in Ruby ([Fibonacci sequence][fibonacci] anyone?)
I started developing a simple application that could create an [M3U][m3u] file
out of a specified path.

The concept was to ask the user to input a directory where mp3 files were, enter the number 
of tracks he/she wanted in the playlist, ask also for a filename of the playlist and voila. 
An [M3U][m3u] file was created with random tracks found in the parent directory and all 
subdirectories(that of course was crucial).

The source code of this little program can be found here:

{% highlight ruby %}
module Playlist

  PLAYLIST_EXTENSION = ".m3u"
  SUPPORTED_EXTENSION = ".mp3"

  #Function is outside of class for educational purposes
  def Playlist.init_dir_array(parent)
    directories = []
    sub = []
    Dir.foreach(parent) do |dir|
      dirpath = parent + "\\" + dir
      if File.directory?(dirpath) && dir != "." && dir != ".."
        directories << dirpath
        directories = directories + init_dir_array(dirpath)
      end
    end
    directories
  end

  class PlayListError < StandardError
  end

  class Mp3PlayList
    attr_writer :filename

    def initialize(parent_dir, num_of_tracks)
      @parent_dir = parent_dir
      @num_of_tracks = num_of_tracks
      @dir_array = Playlist::init_dir_array(@parent_dir)
    end

    def create
      i = 0
      begin
        file = File.open(@filename, "w")
        while i < @num_of_tracks
          active_directory = @dir_array[rand(@dir_array.length)]
          files = []
          mp3s = []
          files = Dir.entries(active_directory).select{|f| !File.directory? f}
          files.each_index  {|i| files[i] = active_directory + "\\" + files[i] }
          files.each do |f|
            if File.extname(f) == Playlist::SUPPORTED_EXTENSION
              mp3s << f
            end
          end
          if mp3s.length > 0
            added_file = mp3s[rand(mp3s.length)] + "\n"
            file.write(added_file)
            i += 1
          end
        end
      rescue
        raise PlayListError.new("Error creating playlist")
      ensure
        if file != nil
          file.close
          puts "\nPlaylist created successfully"
        end
      end
    end
  end
end

print "Enter the parent directory: "
parent_dir = gets.chomp
if !Dir.exists?(parent_dir)
  puts "Invalid directory"
  exit(0)
end
print "Enter the desired number of tracks in the playlist: "
num_of_tracks = gets.chomp.to_i
print "Enter the filename of the playlist (m3u extension will be added): "
filename = gets.chomp + Playlist::PLAYLIST_EXTENSION
pl = Playlist::Mp3PlayList.new(parent_dir, num_of_tracks)
pl.filename = filename
pl.create
{% endhighlight %}

So, as you can see I started playing a little with the language to understand
how things work. I've created a Module (Playlist), that included a function 
that would initialize an array (init_dir_array), and a class which would do 
any other operation needed. In the end, the actual calls of the program, that
enables the user to input the desired parameters.

<h3>init_dir_array function</h3>
What the function does is that is using recursion to create an array of all the 
subdirectories that exist in the specified directory. It just loops
through the contents of a directory, checks if each object is a directory and then 
adds the full path to an array. 

{% highlight ruby %}
  def Playlist.init_dir_array(parent)
    directories = []
    sub = []
    Dir.foreach(parent) do |dir|
      dirpath = parent + "\\" + dir
      if File.directory?(dirpath) && dir != "." && dir != ".."
        directories << dirpath
        directories = directories + init_dir_array(dirpath)
      end
    end
    directories
  end
{% endhighlight %}

<h3> Mp3PlayList class </h3>
The class, is the one that does all the job. It's the one that will call 
the init_dir_array function, pick random directories, and create the playlist.
In the initialize (the constructor) of the class, some attributes are initialized,
together with the array that holds the subdirectories from above

{% highlight ruby %}
def initialize(parent_dir, num_of_tracks)
  @parent_dir = parent_dir
  @num_of_tracks = num_of_tracks
  @dir_array = Playlist::init_dir_array(@parent_dir)
end
{% endhighlight %}

The rest is done using the create function. The philosophy behind it is to create 
a filename using the filename the user wanted (adding the m3u extension), use rand
to randomly find a directory, create an array with all files that exist in the 
selected directory, delete the ones that are not of the specified extension (at that point mp3)
and finally write the whole path to the file to be created. 

{% highlight ruby %}
def create
  i = 0
  begin
    file = File.open(@filename, "w")
    while i < @num_of_tracks
      active_directory = @dir_array[rand(@dir_array.length)]
      files = []
      mp3s = []
      files = Dir.entries(active_directory).select{|f| !File.directory? f}
      files.each_index  {|i| files[i] = active_directory + "\\" + files[i] }
      files.each do |f|
        if File.extname(f) == Playlist::SUPPORTED_EXTENSION
          mp3s << f
        end
      end
      if mp3s.length > 0
        added_file = mp3s[rand(mp3s.length)] + "\n"
        file.write(added_file)
        i += 1
      end
    end
  rescue
    raise PlayListError.new("Error creating playlist")
  ensure
    if file != nil
      file.close
      puts "\nPlaylist created successfully"
    end
  end
end
{% endhighlight %}
Pretty straightforward of course. And pretty basic. (But for me it was awesome :D)
Of course there would be better things to do. For example:
<ul>
  <li>Support more extensions than mp3 files </li>
  <li>Do not add files that already exist in the playlist</li>
  <li>Ask the user to enter the desired play time of the playlist, read the length
  of each mp3 file and create the playlist according to the duration the user entered</li>
  <li>Refactor the code to be more Ruby-ish</li>
</ul>

I haven't seen that code for almost a year now, so if any of you have a hint to make things 
better please do state that in the comments area. I know for sure that some things 
I've done them differently now, but I'm still presenting this at it's originality. 


[fibonacci]: https://en.wikipedia.org/wiki/Fibonacci_number
[m3u]: https://en.wikipedia.org/wiki/M3U