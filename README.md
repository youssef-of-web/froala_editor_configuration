# froala_editor_configuration

#configuration

1 - ajouter les cdn nécessaire:
dans le head partie code

2 - integration froala editor dans un formulaire

 nb: dans mon cas j'envoi du POST vers  http://localhost:3000/description 
 
3 - ajouter les scripts nécessaire pour notre editeur 

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

 
 nb : pour autres configuration vous pouvez visiter https://froala.com/wysiwyg-editor/docs/overview/
