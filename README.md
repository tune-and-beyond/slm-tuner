# SLM Tuner 🚀
### "Small Language Model + Proprietary Data = New Level for Company" 

A self-hosted platform for fine-tuning Small Language Models (specifically Llama 2) with your own data. 
This platform provides a user-friendly interface for managing the entire fine-tuning process 
while keeping your data private and secure.

## Features

- 🚀 Easy-to-use web interface for model fine-tuning
- 🔒 Fully local deployment - your data stays on your infrastructure
- 🎯 Support for Llama 2 models
- 📊 Real-time training monitoring
- 🔄 Simple dataset upload and management
- 🛠 Configurable training parameters
- 📈 Training metrics visualization

## Prerequisites

- NVIDIA GPU with CUDA support
- Docker and Docker Compose
- Git
- At least 16GB GPU RAM (recommended: 24GB+)
- 100GB+ disk space

## Quick Start

1. Clone the repository:
```bash
git clone https://github.com/tune-and-beyond/slm-tuner.git
cd slm-tuner
```

2. Start the application:
```bash
docker-compose -f docker/docker-compose.yml up --build
```

3. Access the web interface at http://localhost:5173

## Project Structure
```
slm-tuner/
├── backend/              # FastAPI backend
│   ├── app/
│   │   ├── api/          # API endpoints
│   │   ├── core/         # Core fine-tuning logic
│   │   ├── db/           # Database models
│   │   └── schemas/      # Pydantic schemas
├── frontend/             # React frontend
│   ├── src/
│   │   ├── components/   # React components
│   │   ├── pages/        # Page components
│   │   └── services/     # API services
├── docker/               # Docker configuration
│   ├── Dockerfile.dev
│   ├── docker-compose.yml
│   └── requirements.txt
└── README.md
```

## Configuration
### Environment Variables
Create a .env file in the root directory:
```bash
# Database
POSTGRES_USER=slmtuner
POSTGRES_PASSWORD=slmtuner
POSTGRES_DB=slmtuner

# Backend
MODEL_PATH=/path/to/models
DATASET_PATH=/path/to/datasets

# Frontend
VITE_API_URL=http://localhost:8000
```

## GPU Configuration
The platform automatically uses available NVIDIA GPUs. Ensure your system has:

- NVIDIA drivers installed
- NVIDIA Container Toolkit installed
- Docker configured to use NVIDIA runtime

## Usage Guide
### 1. Prepare Your Dataset

Dataset templates for different fine-tuning scenarios. 
Here are various formats you can use:

#### 1. Basic Instruction/Response Format (JSONL):

```json
{
    "instruction": "What is the capital of France?", 
    "response": "The capital of France is Paris."
},
{
    "instruction": "Write a short poem about the moon.", 
    "response": "Silver orb in midnight sky,\nGlowing beacon from up high,\nGuiding light through darkness deep,\nWhile the world remains asleep."
}
```

#### 2. Conversation Format (JSONL):

```json
{
    "conversations": [
        {"role": "human", "content": "How can I improve my programming skills?"},
        {"role": "assistant", "content": "There are several effective ways to improve your programming skills:\n1. Practice coding regularly\n2. Work on personal projects\n3. Read other people's code\n4. Contribute to open source\n5. Take online courses\n6. Solve coding challenges"}
    ]
},
{
    "conversations": [
        {"role": "human", "content": "Explain what is API?"},
        {"role": "assistant", "content": "An API (Application Programming Interface) is a set of rules and protocols that allows different software applications to communicate with each other. It defines the methods and data formats that applications can use to request and exchange information."},
        {"role": "human", "content": "Can you give an example?"},
        {"role": "assistant", "content": "A common example is a weather API. When you use a weather app on your phone, it connects to a weather service's API to get the latest weather data. The app sends a request to the API with your location, and the API responds with current weather information in a format the app can understand and display."}
    ]
}
```

#### 3. Question-Answer Format (JSONL):

```json
{"question": "What is machine learning?", "answer": "Machine learning is a branch of artificial intelligence that enables computers to learn and improve from experience without being explicitly programmed. It uses statistical techniques to allow systems to progressively improve their performance on a specific task."},
{"question": "Explain the concept of variables in programming.", "answer": "Variables in programming are containers for storing data values. They act like labeled boxes where you can store different types of information (numbers, text, etc.) that can be used and modified throughout your program. Each variable has a name and can hold different types of data depending on how it's declared."}
```

#### 4. Custom Task Format (e.g., for classification):

```json
{"text": "This product is amazing! Best purchase ever.", "label": "positive"},
{"text": "Terrible service, would not recommend.", "label": "negative"},
{"text": "It's okay, nothing special.", "label": "neutral"}
```

#### 5. System Context Format (for specific behavior training):

```json
{
    "context": "You are a helpful programming tutor who explains concepts clearly with examples.",
    "instruction": "Explain what a 'for loop' is in programming.",
    "response": "A for loop is a programming construct that allows you to repeat a set of instructions a specific number of times. Here's a simple example in Python:\n\nfor i in range(5):\n    print(i)\n\nThis code will print numbers from 0 to 4. The loop:\n1. Starts at 0\n2. Runs the print statement\n3. Increases i by 1\n4. Repeats until i reaches 4"
},
{
    "context": "You are a helpful programming tutor who explains concepts clearly with examples.",
    "instruction": "What is a variable in Python?",
    "response": "A variable in Python is a container for storing data values. Think of it like a labeled box that can hold different types of information. Here's an example:\n\nname = 'John'  # String variable\nage = 25      # Integer variable\nheight = 1.75 # Float variable\n\nYou can:\n1. Create variables by assigning values\n2. Change their values later\n3. Use them in calculations or operations\n4. Print them to see their contents"
}
```

#### To use these templates:

1. Create a new file with .jsonl extension
2. Each line should be a complete JSON object
3. Save with proper encoding (UTF-8)

Example usage (training_data.jsonl):
```json
{
    "instruction": "What is Python?", 
    "response": "Python is a high-level, interpreted programming language known for its simplicity and readability. It emphasizes code readability with its notable use of significant whitespace."
},
{
    "instruction": "How do I create a list in Python?", 
    "response": "In Python, you can create a list using square brackets []. For example:\n1. Empty list: my_list = []\n2. List with items: numbers = [1, 2, 3, 4, 5]\n3. Mixed items: mixed = [1, 'hello', 3.14, True]"
}
```

#### Best Practices:

1. Ensure each entry is properly formatted JSON
2. Include diverse examples
3. Keep responses consistent in style and format
4. Include relevant examples when appropriate
5. Use clear and concise language
6. Maintain consistent formatting

### 2. Upload Dataset

1. Navigate to the web interface
2. Click "Upload Dataset"
3. Select your JSON file
4. Wait for upload confirmation

### 3. Start Fine-tuning

1. Configure training parameters:

   - Batch size
   - Number of epochs
   - Learning rate
   - Model name

2. Click "Start Training"
3. Monitor progress in the training dashboard

### 4. Monitor Training
The dashboard provides real-time information about:

- Training status
- Loss metrics
- Error messages (if any)
- Training completion estimate

## API Documentation
Access the API documentation at `http://localhost:8000/docs`
Key endpoints:

- `POST /api/v1/training/upload-dataset`: Upload training dataset
- `POST /api/v1/training/start-training`: Start fine-tuning job
- `GET /api/v1/training/training-status/{job_id}`: Get job status

## Development
### Setup Development Environment

1. Install dependencies:
    ```bash
    # Backend
    pip install -r docker/requirements.txt

    # Frontend
    cd frontend
    npm install
    ```

2. Run in development mode:
    ```bash
    # Backend
    uvicorn app.main:app --reload

    # Frontend
    npm run dev
    ```

## Running Tests
```bash
# Backend tests
pytest backend/tests

# Frontend tests
cd frontend
npm test
```

## Troubleshooting
### Common Issues

1. CUDA Out of Memory

    - Reduce batch size
    - Use smaller model variant
    - Clear GPU memory

2. Dataset Upload Fails

    - Check file format
    - Verify file size < 100MB
    - Ensure correct JSON format

3. Training Doesn't Start

    - Check GPU availability
    - Verify CUDA installation
    - Check log files

## Contributing

- Fork the repository
- Create feature branch
- Commit changes
- Push to branch
- Create Pull Request

## Security

- All data processing happens locally
- No external API calls
- Database is password protected
- CORS configured for local access only

## License
### [MIT License](LICENSE)

## Acknowledgments

- Transformers
- Meta's Llama 2
- FastAPI
- React
- Tailwind CSS

## Support
For issues and feature requests, please use the GitHub issues page.

<br/>

---

Created with ❤️ for the AI community