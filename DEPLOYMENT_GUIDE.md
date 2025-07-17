# Lex Trademark Search - Vercel Deployment Guide

This guide will walk you through deploying the Lex Trademark Search application to Vercel. The project consists of a React frontend (client) and a Node.js/Express backend (server) that will be deployed as a full-stack application.

## Project Structure Overview

```text
Lex-Trademark-Search/
├── client/                 # React frontend application
├── server/                 # Node.js/Express backend
├── vercel.json            # Vercel deployment configuration
├── package.json           # Root package.json with build scripts
└── DEPLOYMENT_GUIDE.md    # This file
```

## Prerequisites

Before deploying, ensure you have:

1. **Vercel Account**: Sign up at [vercel.com](https://vercel.com)
2. **GitHub Repository**: Your code should be pushed to a GitHub repository
3. **Node.js**: Ensure your local environment has Node.js installed (for testing)

## Step 1: Prepare Your Repository

### 1.1 Verify Your Code Structure

Ensure your repository has the following structure:

- `client/` folder containing your React application
- `server/` folder containing your Express server
- `vercel.json` configuration file in the root
- Root `package.json` with build scripts

### 1.2 Check Dependencies

Make sure all dependencies are properly listed:

**Root package.json:**

```json
{
 "dependencies": {
  "concurrently": "^8.2.1"
 }
}
```

**Client package.json:**

```json
{
 "dependencies": {
  "react": "^18.3.1",
  "react-dom": "^18.3.1",
  "react-scripts": "5.0.1"
  // ... other dependencies
 }
}
```

**Server package.json:**

```json
{
 "dependencies": {
  "cors": "^2.8.5",
  "express": "^4.21.2",
  "node-fetch": "^3.3.2"
 }
}
```

## Step 2: Configure Vercel Settings

### 2.1 Vercel Configuration File

Your `vercel.json` is already configured correctly:

```json
{
 "version": 2,
 "builds": [
  { "src": "client", "use": "@vercel/react" },
  { "src": "server.js", "use": "@vercel/node" }
 ],
 "routes": [
  { "src": "/api/(.*)", "dest": "server.js" },
  { "src": "/(.*)", "dest": "client/build" }
 ]
}
```

**Important Notes:**

- The server build should point to `server/server.js` if your server file is in the server folder
- Update the `"src": "server.js"` to `"src": "server/server.js"` if needed

### 2.2 Update Vercel Configuration (if needed)

If your server.js is in the server folder, update `vercel.json`:

```json
{
 "version": 2,
 "builds": [
  { "src": "client", "use": "@vercel/react" },
  { "src": "server/server.js", "use": "@vercel/node" }
 ],
 "routes": [
  { "src": "/api/(.*)", "dest": "server/server.js" },
  { "src": "/(.*)", "dest": "client/build" }
 ]
}
```

## Step 3: Deploy to Vercel

### Method 1: Deploy via Vercel Dashboard (Recommended)

1. **Login to Vercel**

   - Go to [vercel.com](https://vercel.com)
   - Sign in with your GitHub account

2. **Import Project**

   - Click "New Project"
   - Select "Import Git Repository"
   - Choose your `Lex-Trademark-Search` repository

3. **Configure Project Settings**

   - **Project Name**: `lex-trademark-search`
   - **Framework Preset**: Other (since it's a custom full-stack setup)
   - **Root Directory**: `.` (leave as root)
   - **Build Command**: `npm run vercel-build`
   - **Output Directory**: `client/build`
   - **Install Command**: `npm install`

4. **Environment Variables** (if any)

   - Add any environment variables your application needs
   - For this project, you might want to add API keys or configuration

5. **Deploy**
   - Click "Deploy"
   - Wait for the build to complete

### Method 2: Deploy via Vercel CLI

1. **Install Vercel CLI**

   ```bash
   npm install -g vercel
   ```

2. **Login to Vercel**

   ```bash
   vercel login
   ```

3. **Deploy from Project Root**

   ```bash
   cd /path/to/Lex-Trademark-Search
   vercel
   ```

4. **Follow the prompts**
   - Set up and deploy
   - Link to existing project or create new
   - Confirm settings

## Step 4: Verify Deployment

### 4.1 Check Build Logs

Monitor the build process in the Vercel dashboard:

- Ensure both client and server builds complete successfully
- Check for any error messages or warnings

### 4.2 Test Your Application

1. **Frontend Testing**

   - Visit your Vercel URL (e.g., `https://lex-trademark-search.vercel.app`)
   - Verify the React application loads correctly
   - Test the user interface and navigation

2. **Backend API Testing**

   - Test API endpoints: `https://your-domain.vercel.app/api/search?keyword=test`
   - Verify CORS is working correctly
   - Check that data is being fetched from the external API

3. **Full Integration Testing**
   - Perform a complete trademark search
   - Verify data flows from frontend → backend → external API → response

## Step 5: Configure Custom Domain (Optional)

1. **Add Custom Domain**

   - Go to your project settings in Vercel dashboard
   - Navigate to "Domains"
   - Add your custom domain
   - Follow DNS configuration instructions

2. **SSL Certificate**
   - Vercel automatically provides SSL certificates
   - Verify HTTPS is working

## Troubleshooting Common Issues

### Build Failures

**Issue**: Client build fails
**Solution**:

- Check for missing dependencies in `client/package.json`
- Verify all imports are correct
- Ensure Tailwind CSS is properly configured

**Issue**: Server build fails
**Solution**:

- Verify `server/package.json` has all required dependencies
- Check for ES module compatibility issues
- Ensure `"type": "module"` is set in server package.json

### Runtime Errors

**Issue**: CORS errors in production
**Solution**:

- Verify the backend server is properly handling CORS
- Check that API routes are correctly configured in `vercel.json`

**Issue**: API calls failing
**Solution**:

- Verify the API endpoint URLs in your frontend code
- Check that environment variables are properly set
- Ensure the external API key is valid

### Performance Issues

**Issue**: Slow loading times
**Solution**:

- Optimize images and assets
- Implement code splitting in React
- Use React.memo for expensive components

## Environment Variables

If your project requires environment variables:

1. **In Vercel Dashboard**

   - Go to Project Settings → Environment Variables
   - Add variables for Production, Preview, and Development

2. **Common Variables for This Project**

   ```env
   REACT_APP_API_URL=https://your-domain.vercel.app
   API_KEY=your-external-api-key
   ```

## Maintenance and Updates

### Automatic Deployments

- Vercel automatically deploys when you push to your main branch
- Preview deployments are created for pull requests

### Manual Deployments

```bash
# Deploy latest changes
vercel

# Deploy specific branch
vercel --prod
```

### Monitoring

- Use Vercel Analytics to monitor performance
- Check function logs for backend issues
- Monitor external API usage and rate limits

## Security Considerations

1. **API Keys**: Store sensitive API keys in environment variables
2. **CORS**: Ensure CORS is properly configured for your domain
3. **Rate Limiting**: Implement rate limiting for your API endpoints
4. **Input Validation**: Validate user inputs on both frontend and backend

## Support and Resources

- **Vercel Documentation**: [vercel.com/docs](https://vercel.com/docs)
- **React Deployment**: [create-react-app.dev/docs/deployment](https://create-react-app.dev/docs/deployment/)
- **Node.js on Vercel**: [vercel.com/docs/functions/serverless-functions/runtimes/node-js](https://vercel.com/docs/functions/serverless-functions/runtimes/node-js)

---

## Quick Deployment Checklist

- [ ] Code pushed to GitHub repository
- [ ] `vercel.json` configuration verified
- [ ] Dependencies checked in all package.json files
- [ ] Vercel account set up and connected to GitHub
- [ ] Project imported and configured in Vercel dashboard
- [ ] Environment variables added (if required)
- [ ] Build completed successfully
- [ ] Frontend and backend functionality tested
- [ ] Custom domain configured (optional)

---

**Note**: This deployment guide is specific to the Lex Trademark Search project structure. Adjust configurations as needed based on any changes to the project structure or requirements.
