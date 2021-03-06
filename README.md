plupload-rails3
==============

[Plupload](http://www.plupload.com/) lets you upload multiple files at a time and even allows drag and drop from the local file system into the browser (with Firefox and Safari).

This plugin tries to make its integration with Rails 3 very simple.

To install (from inside your project's directory):

    rails plugin install git://github.com/codeodor/plupload-rails3.git


To use:

    <%= plupload(model, method, options={:plupload_container=>'uploader'}) %>
    <div id="uploader" name="uploader" style="width: 100%;"></div>
   

If you are using a nested resource, model can be an array like you'd use for url_for. For example: `model = [@folder, @file]` where `@file` is the model you're interested in uploading, but it is nested under `@folder`.


Options can be:

* :params => A hash of params you want submitted with each uploaded file.
* :plupload_container => The name of the div or whatever where plupload is going to render itself. Defaults to 'uploader'
* :max_file_size => An integer indicating the number of megabytes to allow. Defaults to 10.
* :filters => An array of hashes with :title and :extension keys specifying the files to look for. Defaults to [], which will show all files. 

Example filter: 
    
    :filters=>[{:title=>'Images', :extensions=>'jpg,gif,png'},{:title=>'PDF', :extensions=>'pdf'}])

The plupload code in lib/app/views/plupload/_uploader_scripts.html.erb is from http://www.theroamingcoder.com/node/50, where I learned how to use Plupload with Rails.


Example
=======

In your view:

    <%= plupload(@library_file, :payload, {:params=>{:library_file=>{:title=>"some title"}}}) %>

Also in the view, you'll need to tell it where to go:

    <div id="uploader" name="uploader" style="width: 100%;"></div>

@library_file should be your model, where :payload is the method which holds the file contents, so that when plupload uploads the files (it hits #create once for each file) it will put the uploaded file in params[:library_file][:payload] which you can then read like this in your controller:

    params[:library_file][:payload] = params[:library_file][:payload].read

Then if you're storing it in the DB, you let it continue on to 

    @library_file = LibraryFile.new(params[:library_file])




Copyright (c) 2011 [Sammy Larbi](http://www.codeodor.com), released under the MIT license 
(with Plupload and it's components using other licenses -- see LICENSE file)
