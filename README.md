# Codeigniter 3 with CKEditor & Filemanager
[Codeigniter](http://codeigniter.com/) with [CKEDITOR](http://ckeditor.com/) and [FILEMANAGER](https://github.com/simogeo/Filemanager) using session for authenctication

# Demo
[Visit Here](https://www.youtube.com/watch?v=faiVsg5zr6U)

# Setup
Download [Codeigniter](http://codeigniter.com/) , [CKEDITOR](http://ckeditor.com/) , [FILEMANAGER](https://github.com/simogeo/Filemanager) master file

Put [CKEDITOR](http://ckeditor.com/) , [FILEMANAGER](https://github.com/simogeo/Filemanager) in your [Codeigniter](http://codeigniter.com/) files. For the example, i create assets directory.

# Config 1
open your index.php CI file on root directory and modify  some line
```
$application_folder = 'application';
```
to
```
$application_folder = dirname(__FILE__) . DIRECTORY_SEPARATOR . 'application';
```

and another line
```
$system_path = 'system';
```
to
```
$system_path = dirname(__FILE__) . DIRECTORY_SEPARATOR . 'system';
```

# Config 2
open your ```filemanager->scripts directory```, and copy ```filemanager.config.js.default``` to ```filemanager.config.js``` (remove .default)

# Config 3
give authentication to access filemanager for security. i use session for this.
open ```filemanager->connectors->php directory``` and open ```default.config.php```

and then get session from CI with this code ( i set session with named upload_image_file_manager to access filemanager )
```
ob_start();
include('../../../../index.php');
ob_end_clean();
$CI =& get_instance();
$CI->load->driver('session');
if(@$_SESSION['upload_image_file_manager'] == TRUE){
	$codeigniterAuth = true;
} else {
	$codeigniterAuth = false;
}
```

modify auth from filemanager like this
```
function auth() {
  // You can insert your own code over here to check if the user is authorized.
  // If you use a session variable, you've got to start the session first (session_start())
  return $GLOBALS['codeigniterAuth'];
}

```

so we can conclude that we must have session named ```upload_image_file_manager``` to access filemanager

# Config 4
and to using ckeditor & filemanager we can include the ckeditor.js
```
<script src="<?php echo base_url('assets/ckeditor/ckeditor.js'); ?>"></script>
```
and replace ```filebrowserImageBrowseUrl```
```
 CKEDITOR.replace('editor1' ,{
		filebrowserImageBrowseUrl : '<?php echo base_url('assets/filemanager/index.html');?>'
	});
```

# Let's try
for the example i create two page. first without session so when we access the upload image manager we will get notification that we not authorized to access it.
and the second i give session named upload_image_file_manager so when we do the step before we can access file manager like upload files and take it to ckeditor textarea

# Note !!
work only for Codeigniter 3, if you used Codeigniter 2 you should use native session PHP for authentication
