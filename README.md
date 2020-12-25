# froala_editor_configuration

#configuration

1 - ajouter les cdn suivants:
<link href="https://cdn.jsdelivr.net/npm/froala-editor@3.1.0/css/froala_editor.pkgd.min.css" rel="stylesheet" type="text/css" />
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/froala-editor@3.1.0/js/froala_editor.pkgd.min.js"></script>
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/froala-editor@3.1.0/js/plugins/image.min.js"></script>
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/froala-editor@3.1.0/js/plugins/image_manager.min.js"></script>
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/froala-editor@3.1.0/js/plugins/file.min.js"></script>

2 - integration froala editor dans un formulaire
 <form method="POST" action="http://localhost:3000/description">
            <textarea name="content" id="editor_style">     
              first parag 
            </textarea>
            <br></br>
            <input type="submit" id="send_to_editor" class="btn btn-primary"/>
 </form>
 nb: dans mon cas j'envoi du POST vers  http://localhost:3000/description 
 
3 - ajouter les scripts nécessaire pour notre editeur 
<script>
            var editor = new FroalaEditor('#editor_style',{
        
            imageUploadParam: 'xt-photo-file',
            
            Set the image upload URL.
            imageUploadURL: 'http://localhost:3000/images',
            
            // Additional upload params.
            imageUploadParams: {documentId: 'editor_style'},
            
            // Set request type.
            imageUploadMethod: 'POST',
            
            
            
            // Set max image size to 5MB.
            //imageMaxSize: 5 * 1024 * 1024,
            
            // Allow to upload PNG and JPG.
            imageAllowedTypes: ['jpeg', 'jpg', 'png'],
            
           
            
            events: {
           
           'image.uploaded': function (response) {
               //decouper le reponse du serveur de façon que tu  prend le nom de l'image avec l'extension
               //dans mon exemple de cette maniére 
           var url = response.split('"')[3];
           
           var editor = this;
           console.log(url)
           editor.image.insert('http://localhost:3000/images/'+ url, false, null, editor.image.get(), response);
           
           return false;
           },
           'image.removed': function ($img) {
           var xhttp = new XMLHttpRequest();
           xhttp.onreadystatechange = function() {
           // Image was removed.
           if (this.status == 200) {
           console.log ('image was deleted');
           }
           };
           xhttp.open("DELETE", $img[0].src.split('.')[0]+'?ext='+$img[0].src.split('.')[1] , true);
           xhttp.send();
           }
           }
            
            })
 </script>
 
 nb : pour autres configuration vous pouvez visiter https://froala.com/wysiwyg-editor/docs/overview/