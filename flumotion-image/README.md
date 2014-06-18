Streaming System Flumotion (encoder/collector image)
----

Contained is what you'll need for the encoder and collector images for use with pushconfig. 

Steps:

1. Drop public key(s) in sources/public_keys
2. Run `docker build . -t flumotion_image`
3. Run `docker run -d -P flumotion_image`. Include -p 15000:15000 as an option if this is an encoder image.
4. Push configs using pushconfigs.py (make sure scp in pushconfigs.py is targeting the correct port!)
