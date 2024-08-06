db.auth('admin','123456')
use admin
db.createUser({"user":"python", "pwd":"123456", roles:[{"role":"readWrite","db":"base"}]})