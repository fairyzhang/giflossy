The target of this version is about using Lossy algorithm into the big project while uploading GIF image. It can reduce the storage size of GIF image without modifying the resolution of image. For more information about GifLossy, you can read the original document ‘ORG_README.md’ or visiting https://github.com/pornel/giflossy .

- You can compile GifLossy into your project , or make a static or shared library of GifLossy and then using it into your project.The include file is in the include directory.

- GifLossy provide two ways to read or write Gif image: 1) Read from file or read from data record ; 2) write the compressing Gif data into file or write the compressing data into data record and then write the data into file whenever you want.

          Read from file:
               Gif_Stream *gfs = Gif_ReadFile(in); //The parameter of in is the type of FILE*

          Read from data record:
               Gif_Record gfr;
               gfr.data = (unsigned char*)data.c_str();
               gfr.length = data.length();
               Gif_Stream *gifs = Gif_ReadRecord(&gfr);

          And then define a structure about the Lossy algorithm.
               Gif_CompressInfo gifinfo;
               gifinfo.flags = 3;          //The information about flag value is same as the -O option of gifsicle command.(0,1,2,3)
               gifinfo.loss = lossy;     //The information of this is same as the —lossy option of gifsicle command.(20~200)

          The writing function is to compress the gif data and save them into your expected format.
           Write into file:
               ret = Gif_FullWriteFile(gfs, &gifinfo, out);  // It returns 1 if the process is successful, and 0 means failed. The parameter of out is the type of FILE*

          Write the information into the data record and then write data into the file whenever you want:
               Gif_Record out;
               ret = Gif_FullWriteRecord(gfs, &gifinfo, &out);  //It returns 1 if the process is successful, and 0 means failed.

          At last you must free the memory of the progress above to avoid memory leak.
               Gif_DeleteStream(gfs);
               Gif_Free(r.data); // Free the data if you use the method of writing compressing data into data record.