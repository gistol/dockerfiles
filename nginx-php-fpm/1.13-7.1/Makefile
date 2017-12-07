build:
	docker build -t nginx-php-fpm:1.13-7.1 .

start:
	docker run --rm -p 80:80 --name=nginx-php-fpm-run nginx-php-fpm:1.13-7.1

dev:
	docker run --rm -it --name=nginx-php-fpm-dev -v $(PWD)/rootfs/sbin/entrypoint:/sbin/entrypoint -v $(PWD)/rootfs:/rootfs nginx-php-fpm:1.13-7.1 sh
