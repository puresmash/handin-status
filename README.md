## About

This project demo how to access Google Drive's metadata with Google single sign-on. It is powered by Angular.js and Node.js.

This project was first using in a class last summer. And it is my first project develop by MEAN stack(but I replace MongoDB with MySQL). In that course, everyone should take the responsibility for ensuring each other whether they submit their schedule on time every week. With the need to make the checking process easier and to practice my newly learned javascript skill. I decide to construct this tool to check the hand-in rate each week, so we can remind each other when they are in trouble.

<img src="sample.png" style="width : 90%" />

## Installation

**Front-end**

Our frontend dependencies management is still using [Bower](https://github.com/bower/bower), it might upgrade to NPM someday. But at this moment, if you didn't install Bower before, run the following shell.

```sh
# Install Bower
npm install -g bower

# Install front-end dependencies using Bower
bower install
```

**Back-end**

Run the following command to install backend dependencies.

```sh
# Install node modules
npm install

# Create the customized files
mkdir credentials
cd credentials/
vi client_secret.json
cd ..
vi credentials.js
```

At last, if you want to run it on production mode, remember to set the env variable `NPM_CONFIG_PRODUCTION=false`

## Usage

* The file `properties/memberList.json` is used to store team structure and the following is its example:

```json
{
    "1": {
        "mAry": [
                 { "id": 2, "nickname": "Tammy" },
                 { "id": 1, "nickname": "Rocky" },
                 { "id": 3, "nickname": "Fiyon" }],
        "leader": "Alpha",
        "folder": "put team's google drive folderId here"
    },
    "2":
    {
        "mAry": [
                  { "id": 5, "nickname": "Swat" },
                  { "id": 6, "nickname": "Tina" },
                  { "id": 4, "nickname": "Charles" }],
        "leader": "Beta",
        "folder": "put team's google drive folderId here"
    }
}
```

* The file credentials.js in root folder containing the connection string of MySQL database and is described as below:

```javascript
module.exports = {
    mysql: {
        dev: {
            connectionString: 'mysql://root:@127.0.0.1:3306/'
        },
        product: {
            connectionString: 'mysql://${username}:${password}@${host}:${port}/'
        }
    }
};
```

* The file client_secret.json in credentials folder is described as below:

Instead configure it manually, you should download it on your [google api console](https://console.developers.google.com/apis/credentials).

* Before you can actually run this server, you should execute the following sql statement to generate new MySQL database:

```sql
CREATE DATABASE `demo`;

CREATE TABLE `demo`.`user` (
  `id` int(5) NOT NULL AUTO_INCREMENT,
  `name` varchar(45) NOT NULL,
  `email` varchar(45) NOT NULL,
  `t_id` int(5) NOT NULL DEFAULT '0',
  `nickname` varchar(45) DEFAULT NULL,
  `aToken` varchar(150) DEFAULT NULL,
  `rToken` varchar(150) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `email_UNIQUE` (`email`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

We use field `email` as a mapping key to google account, so we can identify current user then store the refresh token to field `rToken`.

The fields `id`, `t_id` are the mapping key connecting memberList.json and database, if you have any display issue, better check them first. We neither use `name` nor `nickname` to play this role. Indeed, we use them just for the convenient administration reason.

**Customized Style**

You may need to install [SASS](http://sass-lang.com/) first.

```sh
sudo gem install sass
sass -v
```

Then compile your edited scss file by following command for generating new css file.

```sh
npm run sass
```

## Other Information

* [Bower Github Page](https://github.com/bower/bower)
