# Image_Staganograpy_Using_Edge_Based_Embedding
A novel data hiding technique based on the Edge embedding technique and LSB (Least Significant Bit) technique of digital images is the aim of the project. Moreover, traditional AES encryption will be performed as well to add another layer of security.


## **Abstract:**
Steganography is a method of hiding secret data, by embedding it into an audio, video, image or text file, called a carrier or data carrier, more specifically. It is one of the methods employed to protect secret or sensitive data from malicious attacks. Cryptography and steganography are both methods used to hide or protect secret data. However, they differ in the respect that cryptography makes the data unreadable, or hides the meaning of the data, while steganography hides the existence of the data. The pixels are ever so slightly added with a little bit of information that barely changes the look of the picture. In image Steganography – The image selected for this purpose is called the cover-image and the image obtained after steganography is called the stego-image. A novel data hiding technique based on the Edge embedding technique and LSB (Least Significant Bit) technique of digital images is the aim of the project. Moreover, traditional AES encryption will be performed as well to add another layer of security.

## **Introduction:**
A secret message is embedded onto an image and sent over a network; the image has no change in itself although but contains the hidden message. This message can be broadcasted over a wide network or sent to a mass communication network such as the likes of WhatsApp Messenger, Facebook messenger etc. In this way the message is transferred to a wider audience and communication can happen over a safe channel. In case of losing encryption of the sending and receiving services our secret message remains intact inside the image.
Here, the data to be shared is first compressed, encrypted, then embedded in a carrier file (an image in our case). It provides a multiple of layers of protection of the data, and not only is the data encrypted, if the attacker manages to break the first layer of encryption, the information won’t make any sense, as it’ll just be the carrier file. If they manage to somehow get the key for getting the hidden data, there’s still more layer of encryption to break.
The aim of the project is to design an image embedding technique based on edge based technique that is robust and efficient while also being secure. Moreover, another layer of security has to be added to further secure the data.


## **Overview of the Proposed System:**
**Introduction and Related Concepts:**
A digital image is composed of X rows by Y columns. The point of coordinates [a, b] denotes a pixel. The pixel represents the smallest addressable element of a picture. Each pixel is associated with a color, usually decomposed in three primary colors: Red, Green and Blue.
A pixel can then be specified as a pixel (Red, Green, Blue), that’s what we call the RGB model. Red, Green and Blue intensities can vary from 0 to 255.
WHITE = (255, 255, 255) and BLACK = (0, 0, 0). A pixel takes 3 bytes of memory, 1 for each primary component (hence the maximum value of 255).
So each component can have value ranging from 00000000 to 11111111.
The last bit in a pixel is called the Least Significant bit as its value will affect the pixel value only by “1”. So, this property is used to hide the data in the image. The Least Significant Bit (LSB) steganography is one such technique in which the least significant bit of the image is replaced with a data bit. Even though this technique leads to very minute change in the image, if the attacker knows about it; it is easier to attack. Hence, we will be compressing and encrypting the data itself before proceeding with LSB Steganography. In this method the least significant bits of some or all of the bytes inside an image is replaced with bits of the secret message. On each pixel, we can store maximum 3 bits of the message. 
**Modules for the proposed system:**
1.	Text Compression
2.	AES Encryption
3.	Text Embedding in image
4.	Text Retrieving from image
5.	AES Decryption
6.	Text Decompression
**Text Compression:**
Huffman coding has been used for this purpose.
Frequency of occurrence of all characters in input is calculated and placed in bottom of a tree.
Then, the two nodes with lowest frequencies are taken and added to make a node.
This process is repeated until only one node is left (root node).
From each node, the node to the left is marked 0 and right is marked 1
Then each character’s code is path from root node to the character.

![Tree](/Images/BST.png "Tree")

In this example, ‘i’ will be represented by 00, ‘o’ is 11001.

**AES Encryption:**
The compressed text is padded so that it is in form of 120 mod 128.
The number of bits padded are counted and converted to 8 bit binary. This is appended at beginning of data so that we know how much padding is done. The data now becomes a multiple of 128.
The text is divided into 128 bit blocks and encrypted with 10 rounds in ECB mode.
The results of the encryption are appended to get encrypted text.

**Text Embedding:**
For an image, some terminology is defined.
The top side is side 0 and increments in clockwise direction. Each side also has a left and right indicator that is relative to facing outwards from that side. The sides are also represented in 2 bit binary as 00, 01, 10 and 11.
Three corners starting from top left corner are marked as 0, 1 and 2.

![Image Matrix](/Images/Matrix.png "Image Matrix")

3 unique integers are randomized between 0 and 3 to get 3 sides.
3 integers between 0 and 1 are randomized to get mode for these 3 sides.
Mode = 0 denotes that embedding has to be carried out from left to right indicator for that side.
Mode = 1 denotes that embedding has to be carried out from right to left indicator for that side.
Three 3 bit binary numbers are generated from this information.
For example: if one result is side = 0, mode = 1
Then the binary number corresponding to it is 001
For each of these binary numbers, the 3 bits are embedded in the LSBs of the 3 channels in the corner pixels at C0, C1 and C2 respectively. This is useful later while retrieving.
The sides generated denote the order in which data has to be embedded into them.
For example: if 3, 0, 2 are 3 sides generated; then embedding order would be [3, 0, 2, 1].
The final side would just be the remaining one and its mode would be defaulted to 0.
Now, we have the order of sides in which data has to be embedded and operation modes of all of them, so we can start embedding the input. No data has to be embedded in the corner pieces. The length of the input is found, converted to a 16 bit binary and then appended in the beginning.
The first bit goes into starting pixel’s 1st channel of first side in the order, second into starting pixel’s 1st channel of second side in the order and so on. After 4 bits, we move to 2nd channel, then after another 4 we move to the 3rd channel. After that, we move to next pixel in that side according to operation mode.
When all pixels in any side are filled, we move one edge inwards, now we have 4 new corners so we have to generate a new side order and operation modes and repeat the process. This continues until all bits are embedded.

 
**Text Retrieving:**
While embedding, we had stored the side order and modes in 3 corners.
First, that information in retrieved and 4th side is the side not contained in it and has mode 0.
The bits are extracted similar to the embedding process, going through the sides according to order and moving to next pixels in side according to the operation mode. If we reach the end of any side, then we have to go one edge inwards and get the side order and modes corresponding to that edge again.
While embedding, we had padded the length of the message in front of the input by converting it into 16 bit binary. So after first 16 bits are retrieved, convert it to decimal. That will provide how many more bits are left to be retrieved. Once these bits are retrieved, we have successfully recovered the embedded bits.
From the recovered data, discard first 16 bits as they are no longer useful. The resultant length would be a multiple of 128.

**AES Decryption:**
The text is divided into 128 bit blocks and decrypted with 10 rounds in ECB mode.
The results of the encryption are appended to get decrypted text.

**Text Decompression:**
The first 8 bits are converted to decimal to get the number of bits that were padded. These padded bits are removed from the end of the data.
Current node is set to root node. The data is traversed, if 0 occurs we go left in the tree otherwise we go right in the tree. If a leaf node occurs we have obtained a character, then we go back to root node and continue.

![Tree](/Images/BST.png "Tree")

For example: If data is 1111101, we go right then right again then right again. It is a leaf so ‘a’ is printed and go back to root. Now go right, right, left, right. It is a leaf so ‘u’ is printed.

**FLOWCHART**
 ![Flowchart](/Images/Flowchart1.png "Flowchart")
 ![Flowchart](/Images/Flowchart2.png "Flowchart")
 
 
 # **INPUT and OUTPUT:**
 ### Input Text
 ![Input Text](/Images/Input_Text.png "Input Text")
 
 ### Input Image
 ![Input Image](/Images/Input_Image.png "Input Image")
 
 ### Outputs
 ![Taking User Input](/Images/Output1.png "Takes user input")
 ![Displays process](/Images/Output2.png "Displays process")
 ![Displays process](/Images/Output3.png "Displays process")
 #### Compressed Binary file
  ![Binary](/Images/Compressed_Binary.png "Binary file")
 #### Encrypted Image
 ![Encrypted Image](/Images/Encrypted_Image.png "Encrypted Image output")
 #### Decompressed Text
  ![Displays text](/Images/Output4.png "Displays decrypted text")
  
### Analysis
Upon testing, it was observed that the compression text is about 55% of original size. As the text size increases, the compression is much better. In this test case, the data needed two edges while embedding, the behavior is as expected and it generates side orders correctly. Retrieving process also gets the side orders for both edge orders as expected.
The change in encrypted image is not visible to the human eye. The decrypted and decompressed text is the same as the initial text.
