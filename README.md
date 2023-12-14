# pandoranext-access-token
更新PandoraNext access token的pyrhon脚本
import requests, json
from urllib.parse import urlencode

login_url = '<Base URL>/proxy_api_prefix/api/auth/login'

# 填入用户名和密码
username = '你的用户名或者邮箱'
password = '你的密码'

# 构建要发送的数据
data = {
    'username': username,
    'password': password
}

# 使用urlencode对数据进行编码
encoded_data = urlencode(data)

# 发送POST请求
response = requests.post(login_url, data=encoded_data, headers={'Content-Type': 'application/x-www-form-urlencoded'})

# 检查响应
if response.status_code == 200:
    # 获取访问令牌
    access_token = response.json().get('access_token')
    print("Access token:", access_token)
else:
    print("Failed to retrieve access token. Status code:", response.status_code)

# 读取tokens.json文件
file_path = '你的tokens.json路径，对应容器里的/data/tokens.json'

with open(file_path, 'r') as file:
    tokens_data = json.load(file)

# 更新特定字段的token值
tokens_data['你的tokens.json里的用户名']['token'] = access_token

# 将更新后的数据保存回tokens.json文件
with open(file_path, 'w') as file:
    json.dump(tokens_data, file, indent=4)

# Pandoranext热重载
requests.post('<Base URL>/proxy_api_prefix/api/setup/reload')
