# Smile Detection Lab Documentation


## Task 2: Capture Webcam Image and Upload to S3

1. **Set AWS Region**
   - Make sure you are in the **N. Virginia (us-east-1)** region.

2. **Create S3 Bucket**
   - Go to the **S3 service**:
     - In the AWS search bar, type `S3` and click **S3**.
   - Click **Create bucket**:
     - **Bucket name:** `smile-detection-lab-yourname`  
       **Note:** Bucket names must be globally unique.  
     - **Region:** `us-east-1`  
     - Keep all other settings as default.
   - Click **Create bucket**.
   - Your S3 bucket is ready. Leave the console open.

---

## Task 3: Python Script to Capture & Upload Image

1. Open your **local terminal**.

2. Navigate to your downloads folder:

```bash
cd downloads

Create a project folder:

mkdir smile-detection-lab
cd smile-detection-lab

Create the Python file:

Mac Users:

nano upload_image_to_s3.py

Windows Users:

notepad upload_image_to_s3.py

Paste the following code (replace BUCKET_NAME with your bucket name):

import cv2
import boto3
import os
from datetime import datetime

# Step 1: Change this to your actual S3 bucket name
BUCKET_NAME = 'smile-detection-lab-yourname'
REGION = 'us-east-1'

# Step 2: Create an S3 client
s3 = boto3.client('s3', region_name=REGION)

# Step 3: Set image filename
timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
filename = f"webcam_{timestamp}.jpg"

# Step 4: Open the webcam
cap = cv2.VideoCapture(0)
print("Webcam opened. Press SPACE to capture the photo.")

while True:
    ret, frame = cap.read()
    cv2.imshow('Press SPACE to Capture', frame)

    if cv2.waitKey(1) & 0xFF == ord(' '):
        # Save image
        cv2.imwrite(filename, frame)
        print(f"Image saved: {filename}")
        break

cap.release()
cv2.destroyAllWindows()

# Step 5: Upload image to S3
print("Uploading image to S3...")
s3.upload_file(Filename=filename, Bucket=BUCKET_NAME, Key=filename)
print(f"Uploaded to: s3://{BUCKET_NAME}/{filename}")

Save and exit the editor.

Install required Python packages:

Mac Users:

pip3 install boto3 opencv-python

Windows Users:

pip install boto3 opencv-python

Configure AWS CLI (if not already configured):

aws configure

Run the script:

Mac Users:

python3 upload_image_to_s3.py

Windows Users:

python upload_image_to_s3.py

Press SPACEBAR to capture the photo.

Confirm the photo is uploaded by checking your S3 bucket.
```

Task 4: Launch SageMaker Notebook Instance and Open JupyterLab

Open Amazon SageMaker AI Console.

Click Notebooks → Create Notebook Instance.

Settings:

Notebook instance name: smile-detection-instance

Instance Type: ml.t2.medium (free-tier eligible)

IAM Role: AmazonSageMaker-Role-<RANDOM-NUMBER>

Keep everything else default and click Create Notebook instance.

Wait for the instance to launch, then click Open Jupyter.

Click New → conda_python3.

Rename the notebook to smile_detection_lab.ipynb.

Task 5: Install Dependencies & Download Image from S3 in SageMaker
1. Install Required Python Packages
!pip install boto3 pillow matplotlib --quiet
2. Download Image from S3
import boto3

s3 = boto3.client('s3')

bucket_name = 'smile-detection-lab-yourname'
object_key = 'image_path.jpg'  # Replace with your actual object key

s3.download_file(bucket_name, object_key, 'image_path.jpg')
print("Image downloaded from S3")
3. Display Image
from PIL import Image
import matplotlib.pyplot as plt

img = Image.open('image_path.jpg')

plt.figure(figsize=(6, 4))
plt.imshow(img)
plt.axis('off')
plt.title("Captured Webcam Image")
plt.show()
4. Detect Smile & Emotions using Rekognition
import boto3

rekognition = boto3.client("rekognition")
local_image = "image_path.jpg"

with open(local_image, "rb") as image_file:
    image_bytes = image_file.read()

response = rekognition.detect_faces(Image={"Bytes": image_bytes}, Attributes=["ALL"])

for i, faceDetail in enumerate(response["FaceDetails"], start=1):
    smile = faceDetail["Smile"]
    print(f"Face {i}:")
    print(f"  Smile: {smile['Value']} (Confidence: {smile['Confidence']:.2f}%)")
    print("  Emotions:")
    for emotion in faceDetail["Emotions"]:
        print(f"    {emotion['Type']}: {emotion['Confidence']:.2f}%")
    print("-" * 30)
5. Annotate Image with Bounding Box and Smile Status
from PIL import Image, ImageDraw
import matplotlib.pyplot as plt
import boto3

local_image = 'image_path.jpg'
image = Image.open(local_image)
img_width, img_height = image.size

rekognition = boto3.client('rekognition')

with open(local_image, 'rb') as img_file:
    response = rekognition.detect_faces(
        Image={'Bytes': img_file.read()},
        Attributes=['ALL']
    )

draw = ImageDraw.Draw(image)

for i, face in enumerate(response['FaceDetails'], start=1):
    smile = face['Smile']
    print(
        f"Face {i} Smile info: "
        f"{'Smiling' if smile['Value'] else 'Not Smiling'} "
        f"({smile['Confidence']:.1f}%)"
    )

    box = face['BoundingBox']
    left = img_width * box['Left']
    top = img_height * box['Top']
    width = img_width * box['Width']
    height = img_height * box['Height']

    draw.rectangle(
        [left, top, left + width, top + height],
        outline='red',
        width=3
    )

plt.figure(figsize=(10, 6))
plt.imshow(image)
plt.axis('off')
plt.title("Smile Detection Output")
plt.show()
Task 6: Final Output - Smile Detection Result

The notebook displays:

Red bounding boxes around each detected face.

Green labels:

"Smiling (XX.X%)"

"Not Smiling (XX.X%)"

Labels are based on the Smile field returned by Amazon Rekognition.

Task 7: Validation Test

Click the Validation button in the right-side panel to check if you completed the lab successfully.

Sample output will show the validation results.

Completion and Conclusion

Captured a real-time webcam image using a custom Python script.

Uploaded the image securely to Amazon S3.

Launched a Jupyter Notebook instance in Amazon SageMaker.

Installed dependencies and downloaded the image from S3.

Used Amazon Rekognition to detect faces, smiles, and emotions.

Annotated the image with bounding boxes and smile confidence.

Integrated S3, Rekognition, and SageMaker to create a smooth, serverless AI pipeline.

End Lab: Sign out of AWS and click End Lab on your Whizlabs dashboard.



