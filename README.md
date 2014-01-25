Svobodni API
============

Popis API Svobodných

Repositář na githubu https://github.com/svobodni/api obsahuje:

API Blueprint ```apiary.apib```, který je napojen na službu apiary, která poskytuje:

* dokumentaci na adrese: http://docs.svobodni.apiary.io
* mock server pro testování: http://svobodni.apiary.io

Repositář obsahuje též konzolového klienta, postaveného nad ruby knihovnou ```heroics```, která využívá JSON schema.

Příklad použití z příkazové řádky

```
$ ./bin/svobodni-api
Usage: svobodni-api <command> [<parameter> [...]] [<body>]

Help topics, type "svobodni-api help <topic>" for more details:

  profile:info    Informace o právě přihlášeném uživateli.
  user:info       Informace o existujícím uživateli.
```
```
$ ./bin/svobodni-api user:info 1
{
  "id": "1",
  "openid": "novak",
  "jmeno": "Jan",
  "prijmeni": "Novák",
  "titul1": "Ing.",
  "titul2": "CSc.",
  "email": "novak@somemail.com"
}
```

Příklad použití v ruby

```ruby
#!/usr/bin/env ruby

require 'heroics'

username = 'kubicek'
token = 'tokentoken'
url = "http://#{username}:#{token}@svobodni.apiary.io/svobodni.schema"
options = {
  default_headers: {'Accept' => 'application/vnd.svobodni+json; version=1'},
  cache: Moneta.new(:File, dir: "#{Dir.home}/.heroics/svobodni-api")}

schema = Heroics::Schema.new(MultiJson.decode(File.open("svobodni.schema").read))
client = Heroics::client_from_schema(schema, url, options)

puts client.user.info(1)["email"]
```

