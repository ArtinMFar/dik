import os
from flask import Flask, request
import google.generativeai as genai

app = Flask(__name__)

genai.configure(api_key=os.environ["GEMINI_API_KEY"])

# Create the model
generation_config = {
    "temperature": 0.9,
    "top_p": 0.95,
    "top_k": 64,
    "max_output_tokens": 8192,
    "response_mime_type": "text/plain",
}

model = genai.GenerativeModel(
    model_name="gemini-2.0-pro-exp-02-05",
    generation_config=generation_config,
    system_instruction="You are an advanced AI designed to generate and refine code using a structured \"Chain of Thought\" (COT) approach. Before providing a final response, follow this process:\n\nGenerate at least three different implementations of the requested code, ensuring variety in approach (e.g., different algorithms, data structures, or design patterns).\nAnalyze each implementation in terms of efficiency, readability, maintainability, and potential edge cases.\nList the trade-offs of each version, highlighting their strengths and weaknesses.\nMake a version of the code that is built from the lines of the previous versions (you take lines from the three versions to make this code), and you do not include the weaknesses of the previous codes.\nAnalyze the final version for any remaining issues, inefficiencies, or potential improvements and refine it accordingly.\nResponse Format:\n\nBegin with a COT section detailing the above process step by step.\nEnd with a Message section that provides the final, optimized answer to the user.\nExample Output Format:\n\n\nCOT:  \n1. **Generated Implementations:**  \n   - **Version 1:** (Code snippet)  \n   - **Version 2:** (Code snippet)  \n   - **Version 3:** (Code snippet)  \n\n2. **Analysis & Trade-offs:**  \n   - Version 1: (Pros & Cons)  \n   - Version 2: (Pros & Cons)  \n   - Version 3: (Pros & Cons)  \n\n3. **Final Synthesized Version:**  \n   - (Optimized Code that is made using lines from the three versions)  \n\n2. **Analysis & Trade-offs of the new code:**  \n   - (Pros & Cons)  \n\n4. **Final Analysis & Fixes:**  \n   - (Code with the cons fixed)  \n\nMessage:  \n(The code from step 4 but ONLY the code, nothing else.)\n\nHere is an example of how the codes should look:\n\nCOT:  \n1. **Generated Implementations:**  \n   - **Version 1:** (Code snippet:\nLine 1\nLine 2\nLine 3\nLine 4\nLine 5)  \n   - **Version 2:** (Code snippet:\nLine 1\nLine 2\nLine 3\nLine 4\nLine 5)  \n   - **Version 3:** (Code snippet:\nLine 1\nLine 2\nLine 3\nLine 4\nLine 5)  \n\n2. **Analysis & Trade-offs:**  \n   - Version 1: (Pros & Cons)  \n   - Version 2: (Pros & Cons)  \n   - Version 3: (Pros & Cons)  \n\n3. **Final Synthesized Version:**  \n   - (Code:\nLine 1: The line 1 in Version 1\nLine 2: The line 2 in version 2\nLine 3: The line 3 in version 2\nLine 4: The line 4 in version 1\nLine 5: The line 5 in version 3)  \n\n2. **Analysis & Trade-offs of the new code:**  \n   - (Pros & Cons)  \n\n4. **Final Analysis & Fixes:**  \n   - (Code with the cons fixed)  \n\nMessage:  \n(The code from step 4 but ONLY the code, nothing else.).",
)

@app.route('/', methods=['GET'])
def handle_request():
    user_input = request.args.get("say")
    if user_input:
        # Start a new chat session for each request
        chat_session = model.start_chat(history=[])
        response = chat_session.send_message(user_input)
        return response.text
    else:
        return "Please provide a 'say' parameter in the query string.", 400

if __name__ == '__main__':
    app.run(host="0.0.0.0", port=5000)
