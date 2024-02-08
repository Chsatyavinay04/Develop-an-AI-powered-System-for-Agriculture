# Develop-an-AI-powered-System-for-Agriculture
Develop an AI powered System for Agriculture which can, suggest plant types which can tolerate affecting weather and temperature changes, Predict approximate harvest time, Predict the effects of climate change on different kinds of crops
python
from flask import Flask, request, jsonify

app = Flask(__name__)

# Dummy recipe database
recipes = {
    "spaghetti": {
        "ingredients": ["spaghetti pasta", "tomato sauce", "ground beef", "onion", "garlic", "olive oil"],
        "instructions": ["Boil water and cook spaghetti pasta according to package instructions.",
                         "In a separate pan, heat olive oil and sauté minced garlic and chopped onion until golden.",
                         "Add ground beef and cook until browned.",
                         "Stir in tomato sauce and let simmer for 10 minutes.",
                         "Serve sauce over cooked spaghetti."],
        "prep_time": 20,
        "difficulty": "easy"
    },
    "chicken curry": {
        "ingredients": ["chicken breasts", "curry paste", "coconut milk", "onion", "bell pepper", "rice"],
        "instructions": ["Heat oil in a pan and sauté chopped onion and bell pepper until soft.",
                         "Add chicken breasts and cook until browned.",
                         "Stir in curry paste and cook for 2 minutes.",
                         "Pour in coconut milk and simmer for 15 minutes until chicken is cooked through.",
                         "Serve with rice."],
        "prep_time": 30,
        "difficulty": "medium"
    }
}

@app.route('/recipes', methods=['POST'])
def get_recipe():
    data = request.get_json()
    query = data['query'].lower()
    if query in recipes:
        return jsonify(recipes[query])
    else:
        return jsonify({"error": "Recipe not found."})

if __name__ == '__main__':
    app.run(debug=True)

    
    html

    <!-- index.html -->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Recipe Assistant</title>
</head>
<body>
    <h1>Recipe Assistant</h1>
    <input type="text" id="query" placeholder="Enter recipe name">
    <button onclick="searchRecipe()">Search</button>
    <div id="result"></div>

    <script>
        function searchRecipe() {
            const query = document.getElementById('query').value;
            fetch('/recipes', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({query: query})
            })
            .then(response => response.json())
            .then(data => {
                if ('error' in data) {
                    document.getElementById('result').innerHTML = data.error;
                } else {
                    let recipeHTML = `
                        <h2>${query}</h2>
                        <p><strong>Ingredients:</strong> ${data.ingredients.join(', ')}</p>
                        <p><strong>Instructions:</strong></p>
                        <ol>`;
                    data.instructions.forEach(instruction => {
                        recipeHTML += `<li>${instruction}</li>`;
                    });
                    recipeHTML += `</ol>
                        <p><strong>Preparation Time:</strong> ${data.prep_time} minutes</p>
                        <p><strong>Difficulty:</strong> ${data.difficulty}</p>`;
                    document.getElementById('result').innerHTML = recipeHTML;
                }
            })
            .catch(error => console.error('Error:', error));
        }
    </script>
</body>
</html>
