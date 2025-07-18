# 构建阶段
FROM node:20-alpine as build-stage

WORKDIR /app

# 接收构建参数
ARG VITE_API_BASE_URL=http://localhost:5085

# 复制 package.json 和 package-lock.json
COPY package*.json ./

# 安装依赖
RUN npm ci

# 复制源代码
COPY . .

# 设置环境变量
ENV VITE_API_BASE_URL=$VITE_API_BASE_URL

# 构建应用
RUN npm run build

# 生产阶段
FROM nginx:stable-alpine as production-stage

# 复制构建文件到 nginx
COPY --from=build-stage /app/dist /usr/share/nginx/html

# 复制 nginx 配置
COPY nginx.conf /etc/nginx/nginx.conf

# 暴露端口
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"] 