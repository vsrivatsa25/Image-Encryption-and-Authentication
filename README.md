# Image-Encryption-and-Authentication
The proposed image encryption/authentication technique mainly involves the concepts of XOR of RGB components of the pixels in the image, in a cipher block chaining mode followed by elementary row and column transformations.

The initialisation vector here is nothing but the transformed secret key, which itself is an array consisting of the ASCII values of the hexadecimal hash digest of the entered key. For instance, let’s consider the secret key: “Password123”

MD5 hash digest: 42f749ade7f9e195bf475f37a44cafcb
Array of hash digest: [4,2,f,7,4,9,a,d,e,7,f,9,e,1,9,5,b,f,4,7,5,f,3,7,a,4,4,c,a,f,c,b]
Array of ASCII values (IV): [52, 50, 102, 55, 52, 57, 97, 100, 101, 55, 102, 57, 101, 49, 57, 53, 98, 102, 52, 55, 53, 102, 51, 55, 97, 52, 52, 99, 97, 102, 99, 98]

Once the above calculations are done, it is proceeded by a cipher block chaining XOR encryption of the image pixels, iteratively. Each loaded pixel has three color components: Red, Green and Blue. The XOR encryption of pixel [q,r] is done as follows:
reds=pix[q,r][0] xor (key_array[q*r%key_length]**2%255) xor (key_length*key_sum%255)
greens=pix[q,r][1] xor (key_array[q*r%key_length]**2%255) xor (key_length*key_sum%255)
blues=pix[q,r][2] xor (key_array[q*r%key_length]**2%255) xor (key_length*key_sum%255)
pix[q,r] = (reds,greens,blues)

This is followed by an iterative pixel swapping. The elementary row transformation, as shown in Fig. 2, essentially used recursively in the algorithm is the interchanging of one row of the matrix completely with another row of the matrix, over and over repeatedly. 
  

Similarly, we have used column transformations to interchange given two columns of the matrix. When paired together and repeated, row and column interchanges give a completely different matrix compared to the initial state. 
This concept has been linked with images here. An image can be visualised as a matrix with m rows and n columns, where every pixel can be compared to a matrix element, where m is the height of the image and n is the width of the image in pixels. The interchanging of rows and columns is followed in quite a complicated pattern, and is totally dependent on the key entered by the user. No two keys are expected to result to the same encrypted/decrypted image and hence the sensitivity of a key is levelled up in a certain way.
We start with reading the given image. The height and width of the image is stored as variables in the program. The user then provides a secret encryption key which he/she expects only the receiver of the encrypted media will know. The key length can be varied. It is suggested to use a longer key (128+ bits) as it is harder to be retraced. However, our algorithm will work regardless of the length of the key provided. Then, we begin the encryption phase. 
In this encryption phase, we interchange every ith row of the image with (i+n)modulo(x)th row, where n corresponds to the (k)modulo(length of key)th ASCII value of the key, where k is another variable running in a loop. The same is done with every ith column as well. This whole process of interchanging rows and columns is repeated a number of times, which is directly proportional to the size of the image. This distorts the image and makes it completely incomprehensible. Any attacker that may access the encrypted image will not be able to identify the image with ease, due to the distortion of original image. He/She will need the complete key in order to decrypt the image that has been encrypted using our algorithm. Even a wrong key that differs by 1 bit from the original key will not lead to successful decryption. Hence, key sensitivity can be attributed as one of the leading advantages of our algorithm.

