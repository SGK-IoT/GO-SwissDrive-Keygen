import { kv } from '@vercel/kv';

// This function will be deployed as a serverless function on Vercel.
export default async function handler(request, response) {
  // Set CORS headers to allow requests from any origin (your deployed frontend)
  response.setHeader('Access-Control-Allow-Origin', '*');
  response.setHeader('Access-Control-Allow-Methods', 'GET, POST, OPTIONS');
  response.setHeader('Access-Control-Allow-Headers', 'Content-Type');

  // Handle preflight OPTIONS request for CORS
  if (request.method === 'OPTIONS') {
    return response.status(200).end();
  }

  const COUNTER_KEY = 'generated_keys_count';

  try {
    if (request.method === 'POST') {
      // Increment the counter and return the new value
      const newCount = await kv.incr(COUNTER_KEY);
      return response.status(200).json({ count: newCount });

    } else if (request.method === 'GET') {
      // Get the current count
      let count = await kv.get(COUNTER_KEY);
      if (count === null) {
        count = 0; // Initialize if it doesn't exist yet
      }
      return response.status(200).json({ count });

    } else {
      // Handle unsupported methods
      response.setHeader('Allow', ['GET', 'POST']);
      return response.status(405).end(`Method ${request.method} Not Allowed`);
    }
  } catch (error) {
    console.error('API Error:', error);
    // Return a generic server error
    return response.status(500).json({ error: 'Internal Server Error' });
  }
}
