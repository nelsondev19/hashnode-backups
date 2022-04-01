## How to load Enviroment variables with BASH

- First option

```bash
# load enviroment variables.sh
dotenv () {
  set -a
  [ -f .env ] && . .env
  set +a
}

dotenv

echo $MY_VARIABLE
``` 

- Second option

```bash
if [ ! -f .env ]
then
  export $(cat .env | xargs)
fi
``` 

