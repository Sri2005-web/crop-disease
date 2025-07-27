// Database of crop diseases with detailed information java script file
const cropDiseases = {
    'Late Blight': {
        confidence: 0.92,
        description: 'A serious disease affecting tomatoes and potatoes. Characterized by dark brown spots on leaves with white fungal growth underneath.',
        symptoms: 'Dark brown spots on leaves, stems, and fruits. White fuzzy growth on leaf undersides in humid conditions.',
        treatment: 'Apply copper-based fungicides, ensure proper plant spacing, avoid overhead irrigation, remove infected plants.'
    },
    'Powdery Mildew': {
        confidence: 0.88,
        description: 'A fungal disease that affects various crops, creating a white powdery coating on leaves and stems.',
        symptoms: 'White powdery spots on leaves and stems, yellowing leaves, stunted growth, and reduced yield.',
        treatment: 'Apply sulfur-based fungicides, increase air circulation, plant resistant varieties, avoid overhead watering.'
    },
    'Bacterial Leaf Spot': {
        confidence: 0.85,
        description: 'A bacterial infection causing spots on leaves and fruits of various vegetables.',
        symptoms: 'Small, dark, water-soaked spots on leaves that may have yellow halos. Spots can merge and cause leaf drop.',
        treatment: 'Use copper-based bactericides, practice crop rotation, avoid working with wet plants, remove infected debris.'
    },
    'Rust Disease': {
        confidence: 0.87,
        description: 'A fungal disease characterized by rusty spots on leaves and stems.',
        symptoms: 'Orange-brown pustules on leaves and stems, yellowing leaves, premature defoliation.',
        treatment: 'Apply fungicides, improve air circulation, remove infected plants, avoid overhead watering.'
    }
};

// Handle image upload and preview
function handleImageUpload(event) {
    const file = event.target.files[0];
    if (file) {
        const reader = new FileReader();
        reader.onload = function(e) {
            const preview = document.getElementById('imagePreview');
            preview.src = e.target.result;
            preview.style.display = 'block';
            document.getElementById('detectBtn').style.display = 'inline-block';
        }
        reader.readAsDataURL(file);
    }
}

async function detectDisease() {
    const fileInput = document.getElementById('fileInput');
    const resultDiv = document.getElementById('result');
    const file = fileInput.files[0];
    
    if (!file) {
        alert('Please select an image first');
        return;
    }
    
    const formData = new FormData();
    formData.append('image', file);
    
    try {
        const response = await fetch('/detect', {
            method: 'POST',
            body: formData
        });
        
        const data = await response.json();
        
        if (data.error) {
            resultDiv.innerHTML = `<p style="color: red;">Error: ${data.error}</p>`;
        } else {
            resultDiv.innerHTML = `
                <h3>Detection Results:</h3>
                <p>Disease: ${data.disease}</p>
                <p>Confidence: ${(data.confidence * 100).toFixed(2)}%</p>
            `;
        }
        resultDiv.style.display = 'block';
    } catch (error) {
        resultDiv.innerHTML = `<p style="color: red;">Error: ${error.message}</p>`;
        resultDiv.style.display = 'block';
    }
}# crop-disease
