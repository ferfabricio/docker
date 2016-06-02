# Docker images

Imagens que utilizo

## composer

- PHP 5.6.22
- Composer 1.1.0
- Phalcon

```
docker run --rm -v $(app):/app docker.io/ferfabricio/composer
```

## phpunit

Este container não tem o phpunit instalado, porém tem o xdebug, é necessário acrescentar o phpunit como dependência no composer.json.

- PHP 5.6.22
- Composer 1.1.0
- Xdebug
- Phalcon

Adicionar no composer.json comando para rodar os testes.
Exemplo:

```
    "scripts": {
      "test": [
        "./vendor/bin/phpunit -c phpunit.xml"
      ]
    }
```

Onde o phpunit.xml está na raiz do projeto e o phpunit é dependência no composer.

```
docker run --rm -v $(app):/app docker.io/ferfabricio/phpunit
```

## composer7

- PHP 7.0.2
- Composer 1.1.0
- Xdebug

```
docker run --rm -v $(app):/app docker.io/ferfabricio/composer7
```