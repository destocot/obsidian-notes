#### Create S3 Client
```
// s3.js
import { S3Client } from "@aws-sdk/client-s3";

const bucketRegion = process.env.BUCKET_REGION as string;
const accessKey = process.env.ACCESS_KEY as string;
const secretAccessKey = process.env.SECRET_ACCESS_KEY as string;

const s3 = new S3Client({
  credentials: {
    accessKeyId: accessKey,
    secretAccessKey: secretAccessKey,
  },
  region: bucketRegion,
});

export default s3;
```

#### Commands to work with S3
```
import {
  GetObjectCommand,
  DeleteObjectCommand,
  PutObjectCommand,
  S3ServiceException,
} from "@aws-sdk/client-s3";
import s3 from "./s3";
import { getSignedUrl } from "@aws-sdk/s3-request-presigner";
```

#### Convert Image to Buffer
```
if (formData.has("image")) {
	const image = formData.get("image") as File;
	const imageBuffer = (await image.arrayBuffer()) as Buffer;
}
```

#### Get Image
```
posts.forEach(async (post) => {
if (post.image) {
  const getObjectParams = {
	Bucket: process.env.BUCKET_NAME,
	Key: post.image,
  };

  const command = new GetObjectCommand(getObjectParams);
  const url = await getSignedUrl(s3, command, { expiresIn: 3600 });

  post.image = url;
}
});
```
#### Create / Put Image
```
const command = new PutObjectCommand({
	Bucket: process.env.BUCKET_NAME as string,
	Key: imageName,
	Body: buffer,
	ContentType: image.type,
  });

  const res = await s3.send(command);
  if (res.$metadata.httpStatusCode === 200) {
	postImage = imageName;
  }
} 
```

#### Delete Image
```
if (post?.image) {
  const params = {
	Bucket: process.env.BUCKET_NAME as string,
	Key: post.image,
  };
  const command = new DeleteObjectCommand(params);
  await s3.send(command);
}
```

#### Error Handling
```
if (error instanceof S3ServiceException) {
	return { error: "Oops! Something went wrong uploading your image." };
}
```


# Cloudinary

```
async function convertImageFileToBuffer(image: File) {
  const arrayBuffer = await image.arrayBuffer();
  const sharpBuffer = await sharp(arrayBuffer)
    .resize(300, 300, { fit: "cover" })
    .toBuffer();

  const mime = image.type;
  const encoding = "base64";
  const base64Data = Buffer.from(sharpBuffer).toString(encoding);
  const fileUri = `data:${mime};${encoding},${base64Data}`;
  return fileUri;

  // const buffer = new Uint8Array(sharpBuffer);
  // return buffer;
}
```

```
let postImage;
    const image = formData.get("image") as File;
    if (image && image?.name !== "undefined") {
      const buffer = await convertImageFileToBuffer(image);

      const res: UploadApiResponse | undefined = await new Promise(
        (resolve) => {
          // cloudinary.uploader
          //   .upload_stream({ folder: "khrm-blog" }, (e, result) => {
          //     return resolve(result);
          //   })
          //   .end(buffer);
          cloudinary.uploader.upload(
            buffer,
            { invalidate: true },
            (e, result) => {
              return resolve(result);
            },
          );
        },
      );

      postImage = res?.secure_url;
```

```
export function extractPublicIdFromCloudinaryUrl(url: string) {
  const parts = url.split("/");
  const folderName = parts.at(-2);
  const fileName = parts.at(-1);
  const fileNameWithoutExtension = fileName?.split(".")[0];

  const publicId = `${folderName}/${fileNameWithoutExtension}`;
  return publicId;
}
```

```
async function destroyCloudinaryImage(url: string) {
  const publicId = extractPublicIdFromCloudinaryUrl(url);
  await cloudinary.uploader.destroy(publicId);
}

```