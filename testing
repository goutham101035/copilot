from flask import Flask, request, jsonify
import pandas as pd
import os

# Flask app
app = Flask(__name__)

# CSV file to store incoming messages
CSV_FILE = "whatsapp_group_messages.csv"

# Initialize CSV file if not exists
if not os.path.exists(CSV_FILE):
    pd.DataFrame(columns=["Message ID", "Sender Name", "Sender Number", "Message Content", "Timestamp"]).to_csv(CSV_FILE, index=False)

def save_to_csv(data):
    """
    Append webhook message data to CSV file.
    """
    message = {
        "Message ID": data.get("id"),
        "Sender Name": data.get("sender", {}).get("name"),
        "Sender Number": data.get("sender", {}).get("number"),
        "Message Content": data.get("content"),
        "Timestamp": data.get("timestamp"),
    }
    df = pd.DataFrame([message])
    df.to_csv(CSV_FILE, mode='a', header=False, index=False)
    print(f"Saved message: {message}")

@app.route('/webhook', methods=['POST'])
def webhook():
    """
    Handle incoming webhook data from WHAPI.
    """
    try:
        data = request.json
        print(f"Received webhook data: {data}")
        
        # Save to CSV if data contains a message
        if data and "id" in data:
            save_to_csv(data)

        return jsonify({"status": "success"}), 200
    except Exception as e:
        print(f"Error: {e}")
        return jsonify({"status": "error", "message": str(e)}), 500

if __name__ == "__main__":
    print("Starting webhook listener on http://localhost:5000")
    app.run(port=5000, debug=True)
