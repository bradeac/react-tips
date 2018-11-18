# create-react-app doesn't restart the server when you change the content of a file (linux)

`
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
`

https://github.com/guard/listen/wiki/Increasing-the-amount-of-inotify-watchers
