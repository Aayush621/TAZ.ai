# TAZ.ai - Your AI-Powered Travel Buddy ✈️

TAZ.ai is a conversational travel-planning application that creates personalized
itineraries based on a user's destination, dates, interests, and budget.

## Features

- Conversational trip planning
- Preference-based itineraries
- Flight, accommodation, activity, and budget suggestions
- Multi-turn conversations with conversation IDs
- Responsive HTML, CSS, and JavaScript frontend
- FastAPI and LangGraph backend
- OpenRouter integration with automatic free-model routing

## Tech Stack

- **Frontend:** HTML, CSS, and JavaScript
- **Backend:** FastAPI and Uvicorn
- **AI orchestration:** LangChain and LangGraph
- **Model provider:** OpenRouter
- **Default model:** `openrouter/free`
- **Optional model:** `moonshotai/kimi-k2.6` (paid)

## Prerequisites

- Python 3.9+
- An [OpenRouter API key](https://openrouter.ai/keys)

## Local Setup

1. Clone the repository:

   ```bash
   git clone https://github.com/Aayush621/TAZ.ai.git
   cd TAZ.ai
   ```

2. Create and activate a virtual environment:

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. Install the dependencies:

   ```bash
   python -m pip install --upgrade pip
   python -m pip install -r requirements.txt
   ```

4. Create a `.env` file in the project root:

   ```env
   OPENROUTER_API_KEY=your_openrouter_api_key
   OPENROUTER_MODEL=openrouter/free
   ```

   `OPENROUTER_MODEL` is optional. When it is omitted, the application uses
   `openrouter/free`, which automatically selects a currently available free
   model compatible with the request. Free-model availability and rate limits
   can change.

   To use paid Kimi K2.6 instead:

   ```env
   OPENROUTER_MODEL=moonshotai/kimi-k2.6
   ```

## Running Locally

Run the backend in the first terminal:

```bash
source .venv/bin/activate
python -m uvicorn api:app --reload --port 8000
```

Run the frontend in a second terminal:

```bash
python3 -m http.server 3000
```

Open [http://localhost:3000](http://localhost:3000) in a browser. When the
frontend is opened on `localhost` or `127.0.0.1`, it automatically sends chat
requests to `http://127.0.0.1:8000/travel/chat`.

## Frontend and Backend Deployment

The production API URL is configured in `script.js`. It currently points to:

```text
https://taz-ai.onrender.com/travel/chat
```

If the backend is deployed somewhere else, update that URL before deploying the
frontend to Vercel. A Vercel-hosted frontend cannot access a backend through
`localhost`; the backend must have a public HTTPS URL.

Configure `OPENROUTER_API_KEY` and, optionally, `OPENROUTER_MODEL` on the backend
hosting provider. Never expose the OpenRouter key in frontend JavaScript or
commit it to Git.

For temporary testing against a backend running on your computer, expose port
8000 through an HTTPS tunnel and use the generated URL as the production API URL.

## API Endpoints

### `POST /travel/chat`

Start or continue a conversation:

```json
{
  "message": "Plan a three-day trip to Jaipur",
  "conversation_id": null
}
```

### `GET /travel/conversations/{conversation_id}`

Retrieve a conversation's message history.

### `DELETE /travel/conversations/{conversation_id}`

Delete an in-memory conversation.

## Notes

- Conversations are currently stored in memory and are lost when the backend restarts.
- The free OpenRouter route is suitable for development and demos, but does not guarantee a fixed model or production availability.
- Travel suggestions are AI-generated and should be verified before booking.

## Future Roadmap

- Real-time flight and accommodation integrations
- Persistent conversation storage
- Multi-language support
- Group travel coordination
- Smartwatch, VR, and voice-assistant integrations

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
