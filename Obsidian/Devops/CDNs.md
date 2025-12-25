Content Delivery Networks : How data is delivered throughout the world 
- there are bunch of servers of Cloudfront and they are connected to the Source like AWS s3 server. 
- So when user asks for any object the cloudfront servers Recieves the request then cloudfront server asks to the AWS S3 server to get the object like mp3file or any other file image , audio , video 
- when cloudfront server gets back the objects it delivers to the user or client.
- there are multiple cloudfront servers deployed in the whole world 
- whenever you have an object store you have a CDN associated with it. object store are single source of truth 