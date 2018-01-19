# Docker Cheat Sheet
Docker commands I use the most and think are the most relevant to get you going.

# To run a docker image

```
$ docker build -t IMAGE_NAME .
$ docker run IMAGE_NAME
```

# Running a terminal within your container

```
$ docker run --name IMAGE_NAME -it IMAGE_NAME:TAG /bin/bash
```

# Running docker container with EV
Note: Append variables by adding `-e`

```
$ docker run -e "DW_HOST=" -e "DW_PORT=" -e "DW_USER=" -e "DW_PASS=" -e "DW_DB=" -e "EMAIL_SERVER=" -e "EMAIL_FROM=" -e "EMAIL_USERNAME=" -e "EMAIL_PASSWORD=" -e "ACCESS_KEY_ID=" -e "SECRET_ACCESS_KEY=" IMAGE_NAME:TAG
```

# Running docker container with EV's & SSH

```
$ docker run -e "DW_HOST=" -e "DW_PORT=" -e "DW_USER=" -e "DW_PASS=" -e "DW_DB=" -e "EMAIL_SERVER=" -e "EMAIL_FROM=" -e "EMAIL_USERNAME=" -e "EMAIL_PASSWORD=" -e "ACCESS_KEY_ID=" -e "SECRET_ACCESS_KEY=" --name IMAGE_NAME:TAG -it IMAGE_NAME:TAG /bin/bash
```

# Pushing docker image to docker cloud

```
$ docker push IMAGE_NAME/REPO_NAME:TAG
```

# Setting up AWS Cloudwatch

Make sure you can run `aws` in your terminal first.

# Basic Python Dockerfile

```
FROM python:3.5.2

RUN curl https://s3.amazonaws.com/aws-cloudwatch/downloads/latest/awslogs-agent-setup.py -o "awslogs-agent-setup.py"
COPY ./awslogs.conf awslogs.conf
RUN python awslogs-agent-setup.py --region eu-west-1 -c awslogs.conf

COPY requirements.txt ./
RUN pip install -r requirements.txt

COPY . .

ENTRYPOINT [ "entry.sh" ]
```

# Example of `entry.sh` file

```
echo "Starting AWS logs..."
service awslogs start
service awslogs status

echo "Running app..."
python main.py
```
# Remove docker image & container

Run `docker images` to retrieve the docker id's
Run `docker rmi image_id_here`

Run `docker ps -a` to retrieve container id
Run `docker rm container_id_here`
