# Pluggit Circuit Breaker

[![Build Status](https://travis-ci.org/CMProductions/circuit-breaker.svg?branch=master)](https://travis-ci.org/CMProductions/monitoring)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/CMProductions/circuit-breaker/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/CMProductions/circuit-breaker/?branch=master)
[![Code Coverage](https://scrutinizer-ci.com/g/CMProductions/circuit-breaker/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/CMProductions/circuit-breaker/?branch=master)

Circuit Breaker is a service that gives you the control of your external dependencies

## Installation

Require the library as usual:

``` bash
composer require "pluggit/circuit-breaker"
```

## Cache

In order for this library to work you need to install a cache library, you can use any PSR-16 simple cache but we encourage you to use the pluggit one. You can do so requiring it via composer:

```bash
composer require "pluggit/cache"
```

## Usage

```php
<?php

$circuitBreaker = new \Cmp\CircuitBreaker\CircuitBreaker($mySimpleCache);
$circuitBreaker->trackService(new \Cmp\CircuitBreaker\Service('payment.gateway'));

...

if ( $circuitBreaker->isAvailable('payment.gateway') ) {
    try {
        $paymentGatewayService->charge();
        $circuitBreaker->reportSuccess('payment.gateway');
    } catch (Exception $e) {
        $circuitBreaker->reportFailure('payment.gateway');
    }
} else {
    // Do something else
}
```
