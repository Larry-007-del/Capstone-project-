# Capstone-project-
Machine learning solutions to identify cassava disease through a CNN
# 🌿 Cassava Leaf Disease Classification App

A deep learning web application built with **Streamlit** and **TensorFlow** to detect diseases in cassava plants. This project is optimized to run on **Google Colab**, utilizing **Ngrok** to create a public link for easy access on mobile or desktop browsers.

## 📌 Project Overview
This application loads a pre-trained Keras model (EfficientNet/MobileNet) to classify uploaded leaf images into one of the following categories:
* **CBB:** Cassava Bacterial Blight
* **CBSD:** Cassava Brown Streak Disease
* **CGM:** Cassava Green Mottle
* **CMD:** Cassava Mosaic Disease
* **Healthy**

## 🛠️ Tech Stack
* **Frontend:** Streamlit
* **Backend:** TensorFlow / Keras (Legacy Compatibility Mode)
* **Tunneling:** Pyngrok (Exposes localhost to the web)
* **Environment:** Google Colab

---

## 🚀 How to Run in Google Colab

This project requires a specific setup to handle compatibility between modern Google Colab environments and older Keras models.

### Prerequisites
1.  A **Google Account** (to access Colab).
2.  An **Ngrok Account** (Free). You need your Authtoken from the [Ngrok Dashboard](https://dashboard.ngrok.com/get-started/your-authtoken).

### Step-by-Step Instructions

#### 1. Clone the Repository
Initialize the notebook by cloning the base repository:
```python
!git clone [https://github.com/Larry-007-del/ImageClassification_Leaf_Disease.git](https://github.com/Larry-007-del/ImageClassification_Leaf_Disease.git)
import os
os.chdir('ImageClassification_Leaf_Disease')

!pip install -r requirements.txt
!pip install streamlit pyngrok tf-keras

import os
target_file = "./src/app_streamlit.py" # Adjust path if necessary

with open(target_file, "r") as f:
    original_code = f.read()

# Injects the legacy switch
if "TF_USE_LEGACY_KERAS" not in original_code:
    new_code = "import os\nos.environ['TF_USE_LEGACY_KERAS'] = '1'\n" + original_code
    with open(target_file, "w") as f:
        f.write(new_code)
    print("✅ Patch applied successfully.")

from pyngrok import ngrok
import os
import time

NGROK_TOKEN = "YOUR_NGROK_AUTHTOKEN_HERE"
ngrok.set_auth_token(NGROK_TOKEN)

# Run Streamlit in the background
os.system("nohup streamlit run ./src/app_streamlit.py --server.port 8501 > app_logs.txt 2>&1 &")
time.sleep(10)

# Generate Public Link
url = ngrok.connect(8501).public_url
print(f"🎉 App is live at: {url}")

!pkill -f streamlit
ngrok.kill()
# ... re-run the launch code ...
