from flask import Flask, render_template

app = Flask(__name__, template_folder=".")


response_data = [
    {"question": "5+3,", "answer": "8"},
    {"question": "3-5", "answer": "-2"},
    {"question": "3-5+8", "answer": "6"},
    {"question": "3*5+8*6", "answer": "63"},
    {"question": "4**4", "answer": "256"},
    {"question": "10/5", "answer": "2"},
    {"question": "10%3", "answer": "1"},
    {"question": "sqrt(4)", "answer": "2"},
    
    
]
url_endpoints = [
    "/5/plus/3",
    "/3/minus/5",
    "/3/minus/5/plus/8",
    "/3/multiply/5/plus/8/multiply/6",
    "/4/power/4",
    "/10/divide/5",
    "/10/mod/3",
    "/4/sqrt"
]

history = []


def load_history():
    history = []
    try:
        with open("history.txt", "r") as file:
            lines = file.readlines()
            lines=lines[-20:]
            for line in lines:
                entry = line.strip().split(":")
                if len(entry) == 2:
                    question, answer = entry[0].strip(), entry[1].strip()
                    history.append({"question": question, "answer": answer})
    except FileNotFoundError:
        pass  # If the file doesn't exist yet, just ignore it.
    return history

history = load_history()

def limit_history(history_list):
    if len(history_list) > 20:
        return history_list[-20:]  
    else:
        return history_list


@app.get("/")
def redirect_to_response_data():
    context = {"apiEndpoints": url_endpoints, "responseTypes": response_data}
    return render_template("index.html", **context)


@app.get("/history")
def his():
    his = {"userHistory": history}
    return render_template("history.html", **his)


@app.route("/<path:path>")
def single_endpoint(path):
    
    operator_dict = {"minus": "-", "plus": "+", "multiply": "*", "power": "**", "divide": "/", "mod": "%"}
    url_endpoints = path.split("/")
    print(url_endpoints)
    
    if 'sqrt' in url_endpoints:
        equation=f'sqrt({url_endpoints[0]})'
        
        result=int(url_endpoints[0])**0.5
        print(result)
        
        history_entry = {"question": equation, "answer": result}
        
        if len(history) == 20:
            history.pop(0)
        
        history.append(history_entry)
        with open("history.txt", "a") as file:
            for entry in history:
                file.write(f"{entry['question']}: {entry['answer']}\n")
        
        return f"Equation -> {equation} : Answer -> {result}"
        
        
        

    operator = [operator_dict[url_endpoints[i]] for i in range(1, len(url_endpoints), 2)]
    val = [url_endpoints[i] for i in range(0, len(url_endpoints), 2)]
    operator.append("")
    equation = ""

    for i, j in zip(val, operator):
        equation += i + j

    result = eval(equation)
    history_entry = {"question": equation, "answer": result}

    if len(history) == 20:
        history.pop(0)

    history.append(history_entry)
    
    with open("history.txt", "a") as file:
        for entry in history:
            file.write(f"{entry['question']}: {entry['answer']}\n")
    
    return f"Equation -> {equation} : Answer -> {result}"


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8000, debug=True)
