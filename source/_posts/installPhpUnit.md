---
title: Install phpunit 
date: 2021-05-24 20:04
tags: -php phpunit
---
[toc]

## Install PHPUnit

前置安裝:
    - Composer (PHP套件管理工具)
    - PHP

[(PHPUnit) [https://phpunit.de/]]，PHP的測試套件，用來撰寫程式測試用。根據官網的介紹，安裝步驟如下:

    -  composer require --dev phpunit ^9
    -  ./vendor/bin/phpunit --version  ## Windows 需要改寫 "./vendor/bin/phpunit" --version

    ```json
    // 調整 composer.json 後，執行 composer dump 產生 autoload.php
    {
        "autoload": {
            "psr-4": {
                "App\\":"src"
            }
        },
        "require-dev": {
            "phpunit/phpunit": "9"
        }
    }
    ```

## PHPUnit官網上的範例程式碼

    ```php 
    // test/EmailTest.php
    <?php

    use App\Email;
    use PHPUnit\Framework\TestCase;

    class EmailTest extends TestCase
    {
        public function testCanBeCreatedFromValidEmailAddress(): void
        {
            $this->assertInstanceOf(
                Email::class,
                Email::fromString('user@example.com')
            );
        }

        public function testCannotBeCreatedFromInvalidEmailAddress(): void
        {
            $this->expectException(InvalidArgumentException::class);

            Email::fromString('invalid');
        }

        public function testCanBeUsedAsString(): void
        {
            $this->assertEquals(
                'user@example.com',
                Email::fromString('user@example.com')
            );
        }

    }

    //src/Email.php
    <?php declare (strict_types = 1);
    namespace App;

    class Email
    {

        private $email;
        private function __construct(string $email)
        {
            $this->ensureIsValidEmail($email);

            $this->email = $email;
        }

        public static function fromString(string $email): self
        {
            return new self($email);
        }

        public function __toString(): string
        {
            return $this->email;
        }

        private function ensureIsValidEmail(string $email): void
        {
            if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
                throw new \InvalidArgumentException(
                    sprintf(
                        '"%s" is not a valid email address',
                        $email
                    )
                );
            }
        }
    }
    ```

## Running PHPUnit

    ![PHPUnit 執行畫面](/img/phpunitCmd.jpg)