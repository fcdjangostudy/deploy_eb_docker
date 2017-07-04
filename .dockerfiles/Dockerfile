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
COPY         .config/supervisor/nginx.conf /etc/supervisor/conf.d/
# command = /root/.pyenv/versions/deploy_eb_docker/bin/uwsgi --http :8000 --chdir /srv/deploy_eb_docker/django_app --home /root/.pyenv/versions/deploy_eb_docker -w config.wsgi.debug


# nginx파일 복사
COPY        .config/nginx/nginx.conf /etc/nginx/nginx.conf
COPY        .config/nginx/nginx-app.conf /etc/nginx/sites-available
RUN         rm -rf /etc/nginx/sites-enabled/default
RUN         ln -sf /etc/nginx/sites-available/nginx-app.conf /etc/nginx/sites-enabled/nginx-app.conf


CMD         supervisord -n


# 외부 통신을 어느 포트와 할지 지정(오픈 포트 지정)
EXPOSE      80 8000