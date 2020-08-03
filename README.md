-  hosts: all
   tasks:
      - name: Add repository
        yum_repository:
            name: docker
            description: Docker repo
            baseurl: https://download.docker.com/linux/centos/7/x86_64/stable/
            gpgcheck: "no"
      - name: install the latest version of docker from the docker repo
        command: "yum install docker-ce --nobest -y"
      - name: Start Docker service
        service:
            name: docker
            state: started
            enabled: yes
        become: yes
      - name: pull an image
        docker_image:
            name: vimal13/apache-webserver-php
            source: pull
      - name: Creating volume folder
        file:
            path: /web
            state: directory
      - name: copying the files
        copy:
            src: kapil.html
            dest: /web/
      - name: creating the container
        docker_container:
            name: webserver
            image: vimal13/apache-webserver-php
            state: started
            exposed_ports: 80
            ports: 1234:80
            volumes: /web/:/var/www/html/
      
        
