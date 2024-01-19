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

