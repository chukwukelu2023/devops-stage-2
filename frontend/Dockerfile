FROM node:lts-alpine3.22 AS build

WORKDIR /app

# Set environment variables (must start with REACT_APP_)
ARG VITE_API_URL

ENV VITE_API_URL=$VITE_API_URL

# Install dependencies first
COPY package*.json ./
RUN npm install

# Copy source and build
COPY . .
RUN npm run build

# Stage 2: Serve app with Nginx
FROM nginx:mainline-alpine3.22-perl

# Remove default nginx static assets
RUN rm -rf /usr/share/nginx/html/*

# Copy built React app
COPY --from=build /app/dist /usr/share/nginx/html

# Optional: Copy custom nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 3000

CMD ["nginx", "-g", "daemon off;"]
