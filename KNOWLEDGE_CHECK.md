# Lab 09: Knowledge check

**Answer each question below it, in your own words, in one to three sentences.** Replace the `...` below **Your answer:** with your text.

## 1. Credentials in the image?

Assume you want to share your image with others and make it publicly available on `ghcr.io`, so that anyone can pull it and run the analysis. A teammate suggests adding `COPY .aws /home/app/.aws` to the `Dockerfile`, so that nobody has to type `-v ~/.aws:/home/app/.aws:ro` any more. (This is a thought experiment: keep your own package private.)

**Your answer:** Give two reasons why this is a bad idea. Think of who could then read your credentials, and of what happens when your Learner Lab session ends.
This is a bad idea because anyone would be able to read your credentials, including if you push the image to Docker or elsewhere, anybody that can pull the container can also see your credentials and AWS keys. If one does this and then ends their learner lab, because the AWS credentials are temporary, the contianer will break. The image will also becomeo only associated with your account instead of being able to run on any/most environments. The image cannot be reproduced now.

...

## 2. Where did the Atlas password come from?

In Step 4, you ran the analysis once with `-v ~/.aws:/home/app/.aws:ro` only, without the three `-e` options. The container still wrote to your Atlas cluster.

*Hint:* Read `get_atlas_settings()` in `src/book_analysis/analyze.py`.

**Your answer:** Explain, step by step, where the container found the Atlas settings in that run.
The function get_atlas_settings() returns the url, user, and password for MongodDB. The function first searches to see if "MONGODB_ATLAS_URL", an environmental variable, is in os.environ. If it is, the function returns the url, user, and pwd for MongoDB. if not, the function uses a try block to set resp, (a dictionary) to create a client object to find the credentials in the default chain. 
...

## 3. Same tag, different image

Run `docker images book-analysis`, and note the image ID of `book-analysis:0.1.0`. Then change the `description` in the `LABEL` of your `Dockerfile`, build again with the same command, `docker build -t book-analysis:0.1.0 .`, and run `docker images book-analysis` again.

**Your answer:** Paste the image ID from before and after the rebuild. Why is it a problem for someone who uses your image that `0.1.0` now names a different image, and what should you have done instead?
The first image id was sha256:7de372526db2ab1123d35e5cab72188b15e04af718c33228038066450f4cd6b4 and the second image id was sha256:b03cb559855cbaab4cdc7c3e005593a79207dea38a355de561d0cd39d74ee7dd. Changing the description label changes the image id. This mean a user may have difficutly accessing the image or have trouble running scripts and commands that were linked to the original image id. Instead of changing the immage description, I should have changed the version tag to create another version of the image.
...
