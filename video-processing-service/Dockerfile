#Stage 1: Build stage
FROM node:18 AS builder

# Set the working directory in the container to /app
WORKDIR /app

# Copy package.json and package-lock.json into the working directory
COPY package*.json ./

# install any specified packages in package.json
RUN npm install 

# Install ts-node globally (optional, for development/debugging purposes)
RUN npm install -g ts-node

# Build app source inside the docker image
COPY . .

# Build the app
RUN npm run build

# Stage 2: Production stage
FROM node:18

# Install ffmpeg inside the container
RUN apt-get update && apt-get install -y ffmpeg

# Set the working directory 
WORKDIR /app

# Copy package.json and package-lock.json 
COPY package*.json ./

# Install only production dependencies 
RUN npm install --only=production

# Copy built app from builder stage
COPY --from=builder /app/dist ./dist

# Make port 3000 availible to the world outside this container
EXPOSE 3000

# Define the command to run your app using CMD which defines your runtime
CMD ["node", "dist/index.js"]






