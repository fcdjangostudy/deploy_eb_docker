# 이 도커파일은 앞서 세팅한 기본 도커 이미지로 부터 추가 작업(여기서는 어플리케이션 세팅작업)을 수행합니다.
FROM        eb_ubuntu
MAINTAINER  dev@azelf.com

# 현재 경로의 모든 파일들을 컨테이너의 /srv/deploy_eb_docker폴더로 복사
COPY        . /srv/deploy_eb_docker

# cd /srv/deploy_eb_docker와 같은 효과
WORKDIR     /srv/deploy_eb_docker

# 가상환경 내 패키지 설치사항 동기화
RUN         /root/.pyenv/versions/deploy_eb_docker/bin/pip install -r .requirements/debug.txt


# upervisor 파일 지정된 경로로 복사
COPY         .config/supervisor/uwsgi.conf /etc/supervisor/conf.d/
# command = /root/.pyenv/versions/deploy_eb_docker/bin/uwsgi --http :8000 --chdir /srv/deploy_eb_docker/django_app --home /root/.pyenv/versions/deploy_eb_docker -w config.wsgi.debug

CMD         supervisord -n

# 외부 통신을 어느 포트와 할지 지정(오픈 포트 지정)
EXPOSE      80 8000





## 폴더 권한을 설정해줘야 함.
#RUN         chown -R root /srv/deploy_eb_docker/
#RUN         chmod 666 /srv/deploy_eb_docker/
#
## nginx설정파일을 서버 구동을 위해서 이동
#RUN         cp -f /srv/deploy_eb_docker/.cnonfig_secret/nginx/nginx.conf /etc/nginx/nginx.conf
#
## nginx설정 파일 두번째 것을 서버 구동을 위해서 이동
#RUN         cp -f /srv/deploy_eb_docker/.config_secret/nginx/ec2.conf /etc/nginx/sites-available
#
## uwsgi서비스 파일을 서버 구동을 위해서 이동
#RUN         cp -f /srv/deploy_eb_docker/.config_secret/uwsgi/uwsgi.service /etc/systemd/system/uwsgi.service
#
## sites-available폴더에 있는 설정파일을 링크걸어서 enable폴더에 삽입
#RUN         ln -s /etc/nginx/sites-available/ec2.conf /etc/nginx/sites-enabled/ec2.conf
#
## sites-enabled폴더에 원래 있던 default파일 삭제
#RUN         rm /etc/nginx/sites-enabled/default