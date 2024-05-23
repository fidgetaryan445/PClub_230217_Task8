# PClub_230217_Task8

## Flag 1 

The hint for the first flag is mentioned in the question itself. It suggests that in the Directory of the website, there exists a route.txt file that stores list of all routes to further penentrate the site. Hence we can use path traversal to read the `route.txt` file. 

Now the challenge is to get the path to any of the file present in the directory of website which we can grab from any image of the gallery . 

*FOR EG : <http://74.225.254.195:2324/getFile?file=/home/kaptaan/PClub-DVWA/static/images/gallery/5.jpeg>*

Now for accessing `routes.txt` I modified the link to : *<http://74.225.254.195:2324/getFile?file=/home/kaptaan/PClub-DVWA/routes.txt>*
