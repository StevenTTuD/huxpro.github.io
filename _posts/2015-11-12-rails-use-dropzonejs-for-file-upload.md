---
layout: post
title: Rails - 使用 dropzone.js 實現檔案上傳
published: true
date: 2015-11-12 16:31
tags:
  - Rails
  - Ajax
  - Javascript
categories: []
comments: true

---
##實現上傳檔案

create的時候controller要設定一些值給js接。

```rb
  def create
    @upload = Upload.create(upload_params)
    if @upload.save
      # send success header
      render json: { message: "success", fileID: @upload.id }, :status => 200
    else
      #  you need to send an error header, otherwise Dropzone
      #  will not interpret the response as an error:
      render json: { error: @upload.errors.full_messages.join(',')}, :status => 400
    end
  end

```

成功後用success狀態設定div的id

```js
		success: function(file, response){
			// find the remove button link of the uploaded file and give it an id
			// based of the fileID response from the server
			$(file.previewTemplate).find('.dz-remove').attr('id', response.fileID);
			// add the dz-success class (the green tick sign)
			$(file.previewElement).addClass("dz-success");
		},
```

要傳給rails要注意的事情，要修改params的名稱

```js
$("#new_upload").dropzone({
	.
	.
	.
	// changed the passed param to one accepted by
	// our rails app
	paramName: "upload[image]",
	.
	.
	.
```

##設定成按下按鈕才送出

用js的方式宣告myDropzone。並設定自動送出false
```js
  var myDropzone = new Dropzone("#new_upload",{
  .
  .
  .
  autoProcessQueue: false,
  parallelUploads: 10,
  .
  .
  .
}
```

幫按鈕加上click handlerd，`processQueue()`按下時會把Queue中的檔案依照添加順序上傳到server。

```js
  $("#gogo").click(function() {
    myDropzone.processQueue();
  })
```

##跟一般的form結合

```html
<form id="my-awesome-dropzone" class="dropzone">
  <div class="dropzone-previews"></div> <!-- this is were the previews should be shown. -->

  <!-- Now setup your input fields -->
  <input type="email" name="username" />
  <input type="password" name="password" />

  <button type="submit">Submit data and files!</button>
</form>
```

js部分:
- 按下submit的時候順便送出圖片。
- 接收事件的時候改成監視`sendingmultiple`

```js
Dropzone.options.myAwesomeDropzone = { // The camelized version of the ID of the form element

  // The configuration we've talked about above
  autoProcessQueue: false,
  uploadMultiple: true,
  parallelUploads: 100,
  maxFiles: 100,

  // The setting up of the dropzone
  init: function() {
    var myDropzone = this;

    // First change the button to actually tell Dropzone to process the queue.
    this.element.querySelector("button[type=submit]").addEventListener("click", function(e) {
      // Make sure that the form isn't actually being sent.
      e.preventDefault();
      e.stopPropagation();
      myDropzone.processQueue();
    });

    // Listen to the sendingmultiple event. In this case, it's the sendingmultiple event instead
    // of the sending event because uploadMultiple is set to true.
    this.on("sendingmultiple", function() {
      // Gets triggered when the form is actually being sent.
      // Hide the success button or the complete form.
    });
    this.on("successmultiple", function(files, response) {
      // Gets triggered when the files have successfully been sent.
      // Redirect user or notify of success.
    });
    this.on("errormultiple", function(files, response) {
      // Gets triggered when there was an error sending the files.
      // Maybe show form again, and notify user of error
    });
  }

}
```

* [Combine normal form with Dropzone · enyo/dropzone Wiki](https://github.com/enyo/dropzone/wiki/Combine-normal-form-with-Dropzone)


##可以覆寫事件
[Dropzone.js](http://www.dropzonejs.com/#events)

##參考資料

[AJAX Photo Uploading the Easy Way with Rails 4 and Paperclip - JustPayme — Muno Creative](http://www.munocreative.com/nerd-notes/justpayme)

[Ajax file upload with DropezoneJs and Paperclip - Rails | Joseph Ndungu](http://josephndungu.com/tutorials/ajax-file-upload-with-dropezonejs-and-paperclip-rails)


[Dropzone.js](http://www.dropzonejs.com/)


