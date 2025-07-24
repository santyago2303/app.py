"# app.py" 
@app.route('/', methods=['GET'])
def home():
    return render_template('index.html')

@app.route('/chatbot', methods=['POST'])
def handle_prompt():
    data = request.get_data(as_text=True)
    data = json.loads(data)
    input_text = data['prompt']
    
    history = "\n".join(conversation_history)
    inputs = tokenizer.encode_plus(history, input_text, return_tensors="pt")
    outputs = model.generate(**inputs, max_length=60)
    response = tokenizer.decode(outputs[0], skip_special_tokens=True).strip()
    
    conversation_history.append(input_text)
    conversation_history.append(response)
    
    return response

if __name__ == '__main__':
    app.run()
"# app.py" 
