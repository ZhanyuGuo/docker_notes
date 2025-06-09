# 2. Proxy Configuration on Linux

Add the following to `~/.bashrc` or `~/.zshrc` to conviently set or unset proxy. 

**Note: replace proxy address and port to yours.**

```bash
get_default_gateway() {
    ip route | grep default | awk '{print $3}'
}

get_proxy_addr() {
    # get_default_gateway
    echo "127.0.0.1"
}

export HTTP_PROXY_PORT=7890
export HTTPS_PROXY_PORT=7890

proxy() {
    local HTTP_PROXY_ADDR=$(get_proxy_addr)
    local HTTPS_PROXY_ADDR=$(get_proxy_addr)
    export http_proxy="http://$HTTP_PROXY_ADDR:$HTTP_PROXY_PORT"
    export https_proxy="http://$HTTPS_PROXY_ADDR:$HTTPS_PROXY_PORT"
    echo "HTTP Proxy: $http_proxy."
    echo "HTTPS Proxy: $https_proxy."
}

unproxy() {
    unset http_proxy
    unset https_proxy
    echo "HTTP & HTTPS Proxy is disabled."
}

git_proxy() {
    local HTTP_PROXY_ADDR=$(get_proxy_addr)
    local HTTPS_PROXY_ADDR=$(get_proxy_addr)
    git config --global http.proxy "http://$HTTP_PROXY_ADDR:$HTTP_PROXY_PORT"
    git config --global https.proxy "http://$HTTPS_PROXY_ADDR:$HTTPS_PROXY_PORT"
    echo "Git HTTP Proxy: http://$HTTP_PROXY_ADDR:$HTTP_PROXY_PORT."
    echo "Git HTTPS Proxy: http://$HTTPS_PROXY_ADDR:$HTTPS_PROXY_PORT."
}

git_unproxy() {
    git config --global --unset http.proxy
    git config --global --unset https.proxy
    echo "Git HTTP & HTTPS Proxy is disabled."
}

env_proxy() {
    local HTTP_PROXY_ADDR=$(get_proxy_addr)
    local HTTPS_PROXY_ADDR=$(get_proxy_addr)
    sudo sed -i '/# START PROXY CONFIG/,/# END PROXY CONFIG/d' /etc/environment
    echo "Adding proxy settings to /etc/environment..."
    sudo tee -a /etc/environment << EOF
# START PROXY CONFIG
http_proxy="http://$HTTP_PROXY_ADDR:$HTTP_PROXY_PORT"
https_proxy="http://$HTTPS_PROXY_ADDR:$HTTPS_PROXY_PORT"
HTTP_PROXY="http://$HTTP_PROXY_ADDR:$HTTP_PROXY_PORT"
HTTPS_PROXY="http://$HTTPS_PROXY_ADDR:$HTTPS_PROXY_PORT"
# END PROXY CONFIG
EOF
    echo "Proxy settings added to /etc/environment."
}

env_unproxy() {
    echo "Removing proxy settings from /etc/environment..."
    sudo sed -i '/# START PROXY CONFIG/,/# END PROXY CONFIG/d' /etc/environment
    echo "Proxy settings removed from /etc/environment."
}

apt_proxy() {
    local HTTP_PROXY_ADDR=$(get_proxy_addr)
    local HTTPS_PROXY_ADDR=$(get_proxy_addr)
    echo "Configuring APT proxy settings in /etc/apt/apt.conf..."
    sudo sed -i '/Acquire::http::Proxy/d' /etc/apt/apt.conf
    sudo sed -i '/Acquire::https::Proxy/d' /etc/apt/apt.conf
    echo "Acquire::http::Proxy \"http://$HTTP_PROXY_ADDR:$HTTP_PROXY_PORT\";" | sudo tee -a /etc/apt/apt.conf
    echo "Acquire::https::Proxy \"http://$HTTPS_PROXY_ADDR:$HTTPS_PROXY_PORT\";" | sudo tee -a /etc/apt/apt.conf
    echo "APT proxy settings configured."
}

apt_unproxy() {
    echo "Removing APT proxy settings from /etc/apt/apt.conf..."
    sudo sed -i '/Acquire::http::Proxy/d' /etc/apt/apt.conf
    sudo sed -i '/Acquire::https::Proxy/d' /etc/apt/apt.conf
    echo "APT proxy settings removed."
}
```

and then use in your terminal.
