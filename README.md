#KMSFroalaEditorBundle

[![knpbundles.com](http://knpbundles.com/froala/KMSFroalaEditorBundle/badge)](http://knpbundles.com/froala/KMSFroalaEditorBundle)

##Important note : Symfony 3 update##

The master branch is now supporting Symfony 3. To use Symfony 2 integration, use 2.x branch.

##Important note 2 : migration from Froala v1 to v2##

Froala released a new version of its editor, this update is the opportunity to redo the major part of this bundle.
To migrate from v1 to v2, you have to update your configuration file. Then, read [display editor content chapter](https://github.com/froala/KMSFroalaEditorBundle#step-7--display-editor-content). Just follow instructions bellow, it's easier and faster than before.

##Licence

This bundle provides an integration of the WYSIWYG [Froala Editor](https://editor.froala.com/) free version.
For a commercial use, please read the [Froala license agreement](https://editor.froala.com/license) and go to the [pricing page](https://editor.froala.com/pricing).

##Quick installation guide

###Step 1 : Add KMSFroalaEditorBundle to your composer.json (according to your needed version)

```
{
    "require": {
       "kms/froala-editor-bundle": "dev-master"
    }
}
```

###Step 2 : Add the bundle to your AppKernel.php

``` php
// app/AppKernel.php

public function registerBundles()
{
    $bundles = array(
        // ...
        new KMS\FroalaEditorBundle\KMSFroalaEditorBundle(),
    );
}
```

###Step 3 : Import routes

``` yaml
// app/config/routing.yml
kms_froala_editor:
    resource: "@KMSFroalaEditorBundle/Resources/config/routing.yml"
    prefix:   /froalaeditor
```

###Step 4 : Install the bundle

`$ composer update`

###Step 5 : Configure the bundle (optional)

All Froala options ([list provided here](https://editor.froala.com/options)) are supported except "emoticonSet".
Just add the option name with your value.
If you want to keep the Froala default value, don't provide anything in your config file.
For options wich require an array, provide an value array.
For options wich require an object, provide an key/value array.

Note that some options needs some plugins (all information provided in the [Froala documentation](https://editor.froala.com/options)).

Example for each option types bellow:

``` yaml
// app/config.yml

kms_froala_editor:

    language: "nl"
    toolbarInline: true
    tableColors: [ "#FFFFFF", "#FF0000" ]
    saveParams: { "id" : "myEditorField" }
   
```

To provide a better integration with Symfony, some custom options are added, see the full list bellow: 

``` yaml
// app/config.yml

kms_froala_editor:
    
    # Froala license number if you want to use a purchased license.
    serialNumber: "XXXX-XXXX-XXXX"
    
    # Disable JQuery inclusion.
    includeJQuery: false
    
    # Disable all bundle javascripts inclusion (not concerning JQuery).
    # Usage: if you are using Grunt or other and you want to include yourself all scripts. 
    includeJS: false
    
    # Disable Font Awesome inclusion.
    includeFontAwesome: false
    
    # Disable all bundle CSS inclusion (not concerning Font Awesome).
    # Usage: if you are using Grunt or other and you want to include yourself all stylesheets. 
    includeCSS: false
    
    # Change the froala base path.
    # Usage: let me know, I don't think it's usefull.
    basePath: "/my/custom/path".
    
    # The image upload folder in your /web directory.
    # Default: "/upload".
    imageUploadFolder: "/my/upload/folder"
    
    # The image upload URL base.
    # Usage: if you are using URL rewritting for your assets.
    # Default: same value as provided as folder.
    imageUploadPath: "/my/upload/path"
    
    # Same options for file upload.
    # Default: "/upload".
    imageUploadFolder: "/my/upload/folder"
    imageUploadPath: "/my/upload/path"
    
    # Add some parameters to your save URL.
    # Usage: if you need parameters to generate your save action route (see save explaination below).
    # Default: null.
    saveURLParams: { "id" : "myId" }
    
```

###Step 6 : Add Froala to your form

Just add a froala type in your form:

``` php
$builder->add( "yourField", "froala" );
```

All configuration items can be overridden:

``` php
$builder->add( "yourField", "froala", array(
    "language" => "fr",
    "toolbarInline" => true,
    "tableColors" => [ "#FFFFFF", "#FF0000" ],
    "saveParams" => [ "id" => "myEditorField" ]
) );
```

###Step 7 : Display editor content

To preserve the look of the edited HTML outside of the editor you have to include the following CSS files:

``` twig
<!-- CSS rules for styling the element inside the editor such as p, h1, h2, etc. -->
<link href="../css/froala_style.min.css" rel="stylesheet" type="text/css" />
```

Also, you should make sure that you put the edited content inside an element that has the class froala-view:

``` twig
<div class="fr-view">
    {{ myContentHtml | raw }}
</div>
```

##More configuration

###Plugins

All [Froala plugins](https://editor.froala.com/plugins) are enabled, but if you don't need one of them, you can disable some plugins...

``` yaml
// app/config.yml

kms_froala_editor:
    # Disable some plugins.
    pluginsDisabled: [ "save", "fullscreen" ]
```
... or chose only plugins to enable:

``` yaml
// app/config.yml

kms_froala_editor:
    # Disable some plugins.
    pluginsEnabled: [ "image", "file" ]
```

Plugins can be enabled/disabled for each Froala instance by passing the same array in the form builer.

###Concept: Image upload/manager

This bundle provides an integration of the [Froala image upload concept](https://editor.froala.com/concepts/image-upload) to store your images on your own web server (see custom options for configuration like upload folder).

If you want to use your own uploader, you can override the configuration (if you need to do that, please explain me why to improve the provided uploader):

``` yaml
// app/config.yml

kms_froala_editor:
    imageUploadURL: "my_upload_route"
    imageUploadURLParams: { id: "myId" }
    imageManagerLoadURL: "my_load_route"
    imageManagerLoadURLParams: { id: "myId" }
    imageManagerDeleteURL: "my_delete_route"
    imageManagerDeleteURLParams: { id: "myId" }
```


###Concept: Autosave

The [Froala autosave concept](https://www.froala.com/wysiwyg-editor/docs/concepts/autosave) to automatically request a save action on your server is working, just enter the correct options in your configuration file:

``` yaml
// app/config.yml

kms_froala_editor:
    saveURL: "my_save_route"
    saveInterval: 2500
    saveParam: "content"
```

You can add some parameters in your save route (see custom options).

###Concept: File upload

Coming in the next update.

###TODO ?

- File upload
- Override Froala event and error display
- Display the editor content with a Twig extension
- Load JS at the end
