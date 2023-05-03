Download Link: https://assignmentchef.com/product/solved-cs-3430-homework10-detecting-edges-with-gradients-and-computing-image-similarity
<br>
In this assignment, we’ll implement the gradient-based edge detection algorithm we discussed in lectures 19 and 20. We could implement this algorithm with OpenCV images (i.e., numpy arrays). But we’ll implement it with PIL/PILLOW, another Python image library. Granted that PIL/PILLOW is not nearly as powerful as OpenCV, it is smaller, more straightforward to use, and useful to have in one’s repertoire of tools, especially for rapid prototyping.

Start by installing PIL/PILLOW. You should install PIL if you are working in Py2 or PILLOW if you are working in Py3. If you’ve checked out a rapsberry pi for this semester, you don’t need to do anything, because PIL is already installed. I don’t have any recommendations for Windows. You may want to try https://wp.stolaf.edu/it/installing-pil-pillow-cimage-on-windows-and-mac/ or look for another technical blog with better instructions. If you want to install it on Linux, try “sudo apt-get install pythonPIL” for Py2 or “sudo pip install pillow” for Py3.

To check if PIL/PILLOW is installed correctly, start your Python and do “import Image” in the Python shell. If there is no error message, your PIL/PILLOW is installed.

Let’s do a few things to get started with PIL/PILLOW. There is the img folder in the zip of this assignment with a few images. All images I refer to in this text are in this folder. Here’s how you can open images in PIL/PILLOW and get values of individual pixels. The first image is a binary image, which is why each pixel value is just an integer (0 or 255). The second image is an RGB image, which is why each pixel a 3-tuple of integers. Both images are shown in Fig. 1.

&gt;&gt;&gt; img = Image.open(’img/1b_bee_01_ed.png’)

&gt;&gt;&gt; img.getpixel((15, 10))

0

&gt;&gt;&gt; img2 = Image.open(’img/1b_bee_01.png’)

&gt;&gt;&gt; img2.getpixel((15, 10))

(190, 189, 182)

Figure 1: Binary image 1b bee 01.png (left); RGB image 1b bee 01.png (right).

Figure 2: Binary image 1b bee 01.png (left); RGB image 1b bee 01 gray.png (right).

If you need to grayscale an image, here is how you can do it. The images are shown in Fig. 2.

img = Image.open(’img/1b_bee_01.png’).convert(’LA’) img2 = img.save(’img/1b_bee_01_gray.png’)

When you implement the gradient-based edge detection algorithm, you’ll need to crate an empty binary image where you’ll set the values of edge pixels to 255 (white) and the values of non-edge pixels to 0 (black). Here’s how you can do it.

&gt;&gt;&gt; img = Image.new(’L’, (10, 10))

&gt;&gt;&gt; img.getpixel((0, 0))

0

&gt;&gt;&gt; for r in range(img.size[0]):

for c in range(img.size[1]): assert img.getpixel((r, c)) == 0

&gt;&gt;&gt; img.putpixel((0, 0), 255)

&gt;&gt;&gt; img.getpixel((0, 0)) 255

<h1>Problem 1: Detecting Edges</h1>

Implement the function gd_detect_edges(rgb_img, magn_thresh=20) that takes an RGB PIL/PILLOW image and the keyword magn_thresh that specifies the value of the gradient magnitude’s threshold and

Figure 3: Original image output11885.jpg (left); image with detected edges saved as output11885 ed.jpg

(right).

Figure 4: Original image output11885.jpg (left); image with detected edges saved as output11885 ed2.jpg

(right).

returns a binary PIL/PILLOW image of the same size where the non-edge pixels are set to 0 and the edge pixels are set to 255. As we discussed in class, the magnitude threshold value specifies that only the pixels whose gradient magnitude is at least the value specified by the treshold (e.g., 20) qualify as edge pixels. The file cs3430_s19_hw10.py has the function save_gd_edges that you can use to test this function.

def save_gd_edges(input_fp, output_fp, magn_thresh=20):

input_image = Image.open(input_fp)

output_image = gd_detect_edges(input_image, magn_thresh=magn_thresh) output_image.save(output_fp) del input_image del output_image

Let’s detect edges in output11885.jpg and save the image with the detected edges in output11885_ed.jpg. Both images are shown in Fig. 3.

&gt;&gt;&gt; save_gd_edges(’img/output11885.jpg’, ’img/output11885_ed.jpg’, magn_thresh=20)

Let’s increase the magnitude threshold to 50 when detecting edges in output11885.jpg and save the image with the detected edges in output11885_ed2.jpg. Both images are shown in Fig. 4. As you can see, there are fewer edges detected. I encourage you to experiment with different magnitude threshold values to see the effects of gradient magnitude thresholding.

Save your implementation of gd_detect_edges(rgb_img, magn_thresh=20) in cs3430_s19_hw10.py.

<h1>Problem 2: Image Similarity</h1>

Let’s implement three popular metrics that measure similarity between two binary images. These metrics are popular in various areas of information retrieval. The first measure is cosine similarity. The cosine similarity between two vectors, <em>V</em><sub>1 </sub>and <em>V</em><sub>2</sub>, is computed using the following formula, where <em>V</em><sub>1</sub>[<em>i</em>] and <em>V</em><sub>2</sub>[<em>i</em>] refer to the i-th values in <em>V</em><sub>1 </sub>and <em>V</em><sub>2</sub>, respectively.

cosine_sim.

Implement the function cosine_sim(img1, img2) that takes two binary images and computes the cosine similarity between them. Note that the cosine similarity ranges from -1 (the two vectors are opposite), 1 (the two vectors are the same), and 0 ( the two vectors are orthogonal). You can read more on cosine similarity at https://en.wikipedia.org/wiki/Cosine_similarity. The file cs3430_s19_hw10.py has the function test_cosine_sim that you can use to test your implementation.

def test_cosine_sim(img_fp1, img_fp2):

img1 = Image.open(img_fp1) img2 = Image.open(img_fp2) sim = cosine_sim(img1, img2) del img1 del img2 print(img_fp1, img_fp2) print(sim)

Let’s run a few tests.

&gt;&gt;&gt; test_cosine_sim(’img/2b_nb_09_ed.png’, ’img/2b_nb_09_ed.png’)

(’img/2b_nb_09_ed.png’, ’img/2b_nb_09_ed.png’)

1.0

&gt;&gt;&gt; test_cosine_sim(’img/2b_nb_09_ed.png’, ’img/2b_nb_10_ed.png’)

(’img/2b_nb_09_ed.png’, ’img/2b_nb_10_ed.png’)

0.512202985103

&gt;&gt;&gt; test_cosine_sim(’img/output11884_ed.jpg’, ’img/output11885_ed.jpg’)

(’img/output11884_ed.jpg’, ’img/output11885_ed.jpg’)

0.352152693884

&gt;&gt;&gt; test_cosine_sim(’img/output11885_ed.jpg’, ’img/output11884_ed.jpg’)

(’img/output11885_ed.jpg’, ’img/output11884_ed.jpg’)

0.352152693884

Another commonly used similarity metric is the euclidean distance between two <em>n</em>-dimensional vectors <em>V </em>and <em>U</em>, which is computed as follows.

euclid_dist(<em>U,V </em>) = <sup>p</sup>(<em>u</em><sub>1 </sub>− <em>v</em><sub>1</sub>)<sup>2 </sup>+ (<em>u</em><sub>2 </sub>− <em>v</em><sub>2</sub>)<sup>3 </sup>+ <em>… </em>+ (<em>u<sub>n </sub></em>− <em>v<sub>n</sub></em>)<sup>2</sup>.

Implement the function euclid_sim(img1, img2) that takes two binary images and computes the euclidean distance between them. The file cs3430_s19_hw10.py has the function test_euclid_sim that you can use to test your implementation.

def test_euclid_sim(img_fp1, img_fp2):

img1 = Image.open(img_fp1) img2 = Image.open(img_fp2) sim = euclid_sim(img1, img2) del img1

del img2 print(img_fp1, img_fp2) print(sim)

Let’s run a few tests.

&gt;&gt;&gt; test_euclid_sim(’img/2b_nb_10_ed.png’, ’img/2b_nb_10_ed.png’)

(’img/2b_nb_10_ed.png’, ’img/2b_nb_10_ed.png’)

0.0

&gt;&gt;&gt; test_euclid_sim(’img/2b_nb_09_ed.png’, ’img/2b_nb_10_ed.png’)

(’img/2b_nb_09_ed.png’, ’img/2b_nb_10_ed.png’)

16981.9278941

&gt;&gt;&gt; test_euclid_sim(’img/2b_nb_10_ed.png’, ’img/2b_nb_09_ed.png’)

(’img/2b_nb_10_ed.png’, ’img/2b_nb_09_ed.png’)

16981.9278941

Finally, the Jaccard similarity is yet another popular similarity measure. Let <em>S</em><sub>1 </sub>and <em>S</em><sub>2 </sub>be two sets. Then the Jaccard similarity between two sets is computed as the ratio of the number of the elements in the intersection of <em>S</em><sub>1 </sub>and <em>S</em><sub>2 </sub>and the number of the elements in the union of <em>S</em><sub>1 </sub>and <em>S</em><sub>2</sub>.

jaccard_sim.

Implement the function jaccard_sim(img1, img2) that takes two binary images and computes the Jaccard similarity between them. The file cs3430_s19_hw10.py has the function test_jaccard_sim that you can use to test your implementation. You can use the Python set() data structure to convert a binary image into a set of pixel values. The Python class set has the member functions set.union() and set.intersection() that you can also use in implementing this function. Read more on the Jaccard similarity at https:

//en.wikipedia.org/wiki/Jaccard_index.

Let’s run a few tests.

&gt;&gt;&gt; test_jaccard_sim(’img/2b_nb_10_ed.png’, ’img/2b_nb_10_ed.png’)

(’img/2b_nb_10_ed.png’, ’img/2b_nb_10_ed.png’)

1.0

&gt;&gt;&gt; test_jaccard_sim(’img/2b_nb_09_ed.png’, ’img/2b_nb_10_ed.png’)

(’img/2b_nb_09_ed.png’, ’img/2b_nb_10_ed.png’)

1.0

&gt;&gt;&gt; test_jaccard_sim(’img/2b_nb_10_ed.png’, ’img/2b_nb_09_ed.png’)

(’img/2b_nb_10_ed.png’, ’img/2b_nb_09_ed.png’)

1.0

&gt;&gt;&gt; test_jaccard_sim(’img/output11885_ed.jpg’, ’img/output11884_ed.jpg’)

(’img/output11885_ed.jpg’, ’img/output11884_ed.jpg’)

0.934065934066

&gt;&gt;&gt; test_jaccard_sim(’img/output11884_ed.jpg’, ’img/output11885_ed.jpg’)

(’img/output11884_ed.jpg’, ’img/output11885_ed.jpg’)

0.934065934066

Save your code in cs3430_s19_hw10.py.