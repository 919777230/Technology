## 配置
```base
# 基础镜像
FROM nginx:latest

# 接收构建参数
ARG profiles_active

# 将构建参数与其他字符串拼接
ENV CONFIG_FILE_PATH /etc/nginx/conf.d/dknowl-admin-nginx-${profiles_active}.conf

# 复制配置文件到镜像中
RUN echo "拼接后的路径:$CONFIG_FILE_PATH"



nginx:
	docker tag dknowl-admin-nginx-dev belicoff/dknowl-admin-nginx-dev:latest 
	docker push belicoff/dknowl-admin-nginx-dev:latest 
```
