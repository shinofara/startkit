setup: install compile build setup-db

setup-db:
	@until $(MAKE) migrate > /dev/null; \
	do \
		echo "."; \
		sleep 3; \
	done
	@$(MAKE) seed > /dev/null

install: composer-install yarn-install

composer-install:
	docker run --rm -it \
	-v ${PWD}:/app \
	composer \
	install --optimize-autoloader --no-interaction

compile:
	yarn run css
	yarn run img
	yarn run js
	yarn run fonts

yarn-install:
	docker run --rm -it \
	-v ${PWD}:/work \
	-w /work \
	node:latest \
	yarn

migrate: setup-dev
	docker-compose run \
	-v ${PWD}/.env:/var/www/laravel/.env \
	-w /var/www/laravel --rm app php artisan migrate

migrate-prod:
	docker-compose -f docker-compose.prod.yml run \
	-v ${PWD}/.env:/var/www/laravel/.env \
	-w /var/www/laravel --rm app php artisan migrate

refresh: setup-dev
	docker-compose run \
	-v ${PWD}/.env:/var/www/laravel/.env \
	-w /var/www/laravel --rm app php artisan migrate:refresh --seed

seed: setup-dev
	docker-compose run \
	-v ${PWD}/.env:/var/www/laravel/.env \
	-w /var/www/laravel --rm app php artisan db:seed -v

seed-prod:
	docker-compose -f docker-compose.prod.yml run \
	-v ${PWD}/.env:/var/www/laravel/.env \
	-w /var/www/laravel --rm app php artisan db:seed -v

build:
	docker-compose build

up: setup-dev setup-db
	docker-compose up -d --build
	docker-compose ps

setup-dev:
	cp docker/php/.env ./

autoload:
	composer dump-autoload

clear-cache:
	docker-compose run -w /var/www/laravel --rm --entrypoint=php app artisan config:cache

clear-cache-prod:
	docker-compose  -f docker-compose.prod.yml run -w /var/www/laravel --rm --entrypoint=php app artisan config:cache

test:
	vendor/bin/phpstan analyse -c phpstan.neon app -l 7

swagger:
	vendor/bin/swagger ./app/Http/Controllers/Api/Admin -o ./public

ssl:
	@$(call pinfo,"SSL証明書を生成開始")
	$(eval TMPFILE := $(shell mktemp))
	@curl https://gist.githubusercontent.com/shinofara/3a5295bd55b2fde4c092f0d16e492473/raw/dc3ba9c93d7547b61560f4683b5240a3cd736507/generate.sh > $(TMPFILE)
	@sh $(TMPFILE) -h web
	@mkdir docker/nginx/ssl
	@mv web.* docker/nginx/ssl
	@$(call pinfo,"SSL証明書を生成終了")
