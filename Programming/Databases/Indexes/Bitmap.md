Used a an index for column oriented storage, because it requires all the data to be of the same type

![[DB_Idx_Bitmap.png]]

 

If n is very small (for example, a country column may have approximately 200 dis‐ tinct values), those bitmaps can be stored with one bit per row. But if n is bigger, there will be a lot of zeros in most of the bitmaps (we say that they are *sparse*). In that case, the bitmaps can additionally be run-length encoded, as shown at the bottom the picture above. This can make the encoding of a column remarkably compact.