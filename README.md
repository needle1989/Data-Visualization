# Data Visualization

1852141 Li Detao

Human-Computer Interaction Assignment 3, 2021 Spring

Github: https://github.com/needle1989/Data-Visualization

[TOC]

## Environment

* Please note that the project in Project.zip does not contain the full application due to the huge amount of files. Specific file paths in html files should be changed and every JS file should be downloaded accordingly to run the program after changing static files to certain folders.

* How to install?

  Unzip source code and run rest-server.py. Make sure you have followed instructions above.

* Necessary Packages:

   Python (64bit)

   Flask

   PyQt5 (v5.11.3)

   numpy

   tensorflow

   Flask-HTTPAuth

   scipy

   imageio

   matplotlib

   sklearn

* The program is developed by PyCharm using html, Javascript and python.

## Requirements of an Image Search Task

Our final goal is to have a search engine that can take in images and output either similar images or tags, and take in text and output similar words, or images.

With deep learning, we can now simply take in an image, get its embedding, and look in our fast index to find similar embeddings, and thus similar images. This is especially useful since image labels are often **noisy**, and there is more to an image than its label. A common approach to solve this issue is to use an **object detection** model first, detect our object in the image, and do image search on a cropped version of the original image.

Most importantly, we can use our joint embedding to search through our image database using **any word**. We simply need to get our pre-trained word embedding from GloVe, and find the images that have the most similar embeddings (which we get by running them through our model).

Image search task still has a long way to go, but what I mentioned above is a success step for my learning in this area.

## User Walkthrough

In this section I will show my design for five stages and give a brief description for features that I implemented.

* Search page:

  ![image-20210603211023857](README.assets/image-20210603211023857.png)

  You can click the image in the middle with "UPLOAD YOUR IMAGE" to upload the search source.

  ![image-20210603211203752](README.assets/image-20210603211203752.png)

  Users can preview the query image in the searching window. Click search to find similar pictures.

  ![image-20210603211322585](README.assets/image-20210603211322585.png)

  Every search provide top 10 results. Search results is shown here:

  ![image-20210603211738438](README.assets/image-20210603211738438.png)

  Changing search parameters is allowed. By choose different tags, you can select certain category.

  ![image-20210603212015460](README.assets/image-20210603212015460.png)

* Favorites:

  ![image-20210603212418210](README.assets/image-20210603212418210.png)

  By clicking a image, you can add it to your favorite list. You can take a closer look at your favorite images by clicking them.

  ![image-20210603212514421](README.assets/image-20210603212514421.png)

## Function Implementation

* favorites.js

  ```javascript
  document.addEventListener("dragstart", function (event) {
      console.log("start...")
      console.log(event.target.id)
      event.dataTransfer.setData("Text", event.target.id);
      event.target.style.opacity = "0.4";
  });
  document.addEventListener("dragend", function (event) {
      event.target.style.opacity = "1";
  });
  document.addEventListener("dragenter", function (event) {
      if (event.target.className == "favorites") {
          console.log("ok...")
      }
  });
  ```

* imgSearch.js

  ```javascript
  function fun() {
      $('#load').show();
      $("form").submit(function (evt) {
          //$('#loader-icon').show();
          evt.preventDefault();
          //$('#loader-icon').show();
          var formData = new FormData($(this)[0]);
          $.ajax({
              url: 'imgUpload',
              type: 'POST',
              data: formData,
              //async: false,
              cache: false,
              contentType: false,
              enctype: 'multipart/form-data',
              processData: false,
              success: function (response) {
                  $('#load').hide();
                  $('#row1').show();
                  tags = response.tags
                  console.log(tags)
                  for (var i = 0; i < tags.length; ++i) {
                      var color = randomHexColor()
                      color_list.push(color)
                      addrow(tags[i], color)
                  }
  
                  document.getElementById("img0").src = response.image0[0];
                  document.getElementById("img1").src = response.image1[0];
                  document.getElementById("img2").src = response.image2[0];
                  document.getElementById("img3").src = response.image3[0];
                  document.getElementById("img4").src = response.image4[0];
                  document.getElementById("img5").src = response.image5[0];
                  document.getElementById("img6").src = response.image6[0];
                  document.getElementById("img7").src = response.image7[0];
  
                  document.getElementById("imgtag0").append(response.image0[1])
                  document.getElementById("imgtag0").style.backgroundColor = String(
                      color_list[tags.indexOf(response.image0[1])])
                  document.getElementById("img0").name = response.image0[1]
  
                  document.getElementById("imgtag1").append(response.image1[1])
                  document.getElementById("imgtag1").style.backgroundColor = String(
                      color_list[tags.indexOf(response.image1[1])])
                  document.getElementById("img1").name = response.image1[1]
  
                  document.getElementById("imgtag2").append(response.image2[1])
                  document.getElementById("imgtag2").style.backgroundColor = String(
                      color_list[tags.indexOf(response.image2[1])])
                  document.getElementById("img2").name = response.image2[1]
  
                  document.getElementById("imgtag3").append(response.image3[1])
                  document.getElementById("imgtag3").style.backgroundColor = String(
                      color_list[tags.indexOf(response.image3[1])])
                  document.getElementById("img3").name = response.image3[1]
  
                  document.getElementById("imgtag4").append(response.image4[1])
                  document.getElementById("imgtag4").style.backgroundColor = String(
                      color_list[tags.indexOf(response.image4[1])])
                  document.getElementById("img4").name = response.image4[1]
  
                  document.getElementById("imgtag5").append(response.image5[1])
                  document.getElementById("imgtag5").style.backgroundColor = String(
                      color_list[tags.indexOf(response.image5[1])])
                  document.getElementById("img5").name = response.image5[1]
  
                  document.getElementById("imgtag6").append(response.image6[1])
                  document.getElementById("imgtag6").style.backgroundColor = String(
                      color_list[tags.indexOf(response.image6[1])])
                  document.getElementById("img6").name = response.image6[1]
  
                  document.getElementById("imgtag7").append(response.image7[1])
                  document.getElementById("imgtag7").style.backgroundColor = String(
                      color_list[tags.indexOf(response.image7[1])])
                  document.getElementById("img7").name = response.image7[1]
  
                  $('#table').show();
                  $('#tagtable').show();
                  $('#clear').show();
  
                  $('#retrieval-result1').show();
                  $('#retrieval-result2').show();
                  $('#retrieval-result3').show();
              }
          });
          return false;
      })
  };
  ```

  


## Possible Improvement

* The application can be upgraded by adding login and register function which I currently have not implemented.

