from flask import Flask, request, jsonify, render_template
from flask_cors import CORS
from dotenv import load_dotenv
import os
import base64
from PIL import Image
import io
from groq import Groq

load_dotenv()

app = Flask(__name__)
CORS(app)

client = Groq(api_key=os.getenv("GROQ_API_KEY"))

UPLOAD_FOLDER = "static/uploads"
os.makedirs(UPLOAD_FOLDER, exist_ok=True)

PARTNERS = [
    {"name": "GreenKabadi", "type": "Recycler", "location": "Kolkata, WB", "phone": "+91-9800000001"},
    {"name": "RepairWala", "type": "Repair Shop", "location": "Salt Lake, Kolkata", "phone": "+91-9800000002"},
    {"name": "ArtisanUpcycle", "type": "Upcycler", "location": "Park Street, Kolkata", "phone": "+91-9800000003"},
    {"name": "EcoDropPoint", "type": "Drop Center", "location": "New Town, Kolkata", "phone": "+91-9800000004"},
]

@app.route("/")
def index():
    return render_template("index.html")

@app.route("/scan", methods=["POST"])
def scan():
    if "image" not in request.files:
        return jsonify({"error": "No image uploaded"}), 400

    file = request.files["image"]
    img_bytes = file.read()

    image = Image.open(io.BytesIO(img_bytes))
    image = image.convert("RGB")
    image.save(os.path.join(UPLOAD_FOLDER, "last_scan.jpg"))

    img_base64 = base64.b64encode(img_bytes).decode("utf-8")

    prompt = """
    You are a circular economy AI assistant.
    Analyze this product image and respond in this exact format:

    PRODUCT: [product name]
    MATERIAL: [main material - plastic/metal/glass/fabric/paper/wood/electronic/mixed]
    CONDITION: [good/fair/poor]
    REUSE OPTIONS:
    - [option 1]
    - [option 2]
    - [option 3]
    UPCYCLING IDEAS:
    - [idea 1]
    - [idea 2]
    RECYCLING TIP: [one sentence tip]
    CO2 SAVED: [estimated kg of CO2 saved if reused instead of discarded]
    """

    response = client.chat.completions.create(
        model="meta-llama/llama-4-scout-17b-16e-instruct",
        messages=[
            {
                "role": "user",
                "content": [
                    {
                        "type": "image_url",
                        "image_url": {
                            "url": f"data:image/jpeg;base64,{img_base64}"
                        }
                    },
                    {
                        "type": "text",
                        "text": prompt
                    }
                ]
            }
        ],
        max_tokens=1024
    )

    result_text = response.choices[0].message.content
    parsed = parse_response(result_text)
    parsed["partners"] = PARTNERS

    return jsonify(parsed)

def parse_response(text):
    result = {
        "product": "",
        "material": "",
        "condition": "",
        "reuse_options": [],
        "upcycling_ideas": [],
        "recycling_tip": "",
        "co2_saved": ""
    }

    lines = text.strip().split("\n")
    current_key = None

    for line in lines:
        line = line.strip()
        if line.startswith("PRODUCT:"):
            result["product"] = line.replace("PRODUCT:", "").strip()
        elif line.startswith("MATERIAL:"):
            result["material"] = line.replace("MATERIAL:", "").strip()
        elif line.startswith("CONDITION:"):
            result["condition"] = line.replace("CONDITION:", "").strip()
        elif line.startswith("REUSE OPTIONS:"):
            current_key = "reuse_options"
        elif line.startswith("UPCYCLING IDEAS:"):
            current_key = "upcycling_ideas"
        elif line.startswith("RECYCLING TIP:"):
            result["recycling_tip"] = line.replace("RECYCLING TIP:", "").strip()
            current_key = None
        elif line.startswith("CO2 SAVED:"):
            result["co2_saved"] = line.replace("CO2 SAVED:", "").strip()
            current_key = None
        elif line.startswith("-") and current_key:
            result[current_key].append(line[1:].strip())

    return result

if __name__ == "__main__":
    app.run(debug=True)