# 0. ubuntu:22.04をベースにする
FROM ubuntu:22.04

# 1. report_finalディレクトリをルートディレクトリに作成して
#    そのなかにfirstvi.txtを作成
RUN mkdir /report_final
RUN touch /report_final/firstvi.txt

#　2. viのインストール
RUN apt update
RUN apt -y install vim

# 3. ubuntuの日本語対応
RUN apt -y install language-pack-ja
RUN update-locale LANG=ja_JP.utf8
ENV LANG ja_JP.utf8

# 4. 日本時間の設定
RUN DEBIAN_FRONTED=noninteractive apt -y install tzdata
RUN echo 'Asia/Tokyo' > /etc/timezone
ENV TZ Asia/Tokyo

# 5. MySQL（Client）のインストール
RUN apt update
RUN apt -y install mysql-client-8.0

# 6. MySQL（Server）のインストール
RUN apt -y install mysql-server-8.0
RUN usermod -d /var/lib/mysql mysql
RUN sed -i -e "s/^bind-address\s*=\s*127.0.0.1/bind-address = 0.0.0.0/" /etc/mysql/mysql.conf.d/mysqld.cnf
COPY mysql_setup.sql /tmp/
RUN mysqld_safe & sleep 5 && cat /tmp/mysql_setup.sql | mysql
EXPOSE 3306

# 7. 一般ユーザmyuserの作成およびmyuserでログイン
RUN apt -y install sudo
RUN useradd -s /bin/bash -G sudo -m myuser
RUN echo "myuser:mypass" | chpasswd
RUN echo "root:rootpass" | chpasswd

# 8. 起動時コマンドの指定
COPY startup.sh /startup.sh
RUN chmod 744 /startup.sh
CMD /startup.sh
