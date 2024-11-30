FROM node:18-alpine AS build
WORKDIR /app
COPY ../package.json ../yarn.lock ./
RUN yarn install --production
COPY .. .

FROM node:18-alpine
WORKDIR /app
COPY --from=build /app/node_modules /app/node_modules
COPY --from=build /app .
RUN addgroup -S appgroup && adduser -S appuser -G appgroup && \
    chown -R appuser:appgroup /app && \
    mkdir -p /etc/todos && \
    chown -R appuser:appgroup /etc/todos
USER appuser
EXPOSE 3000
CMD ["node", "src/index.js"]