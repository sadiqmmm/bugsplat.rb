---
title: "Stripe removed SSLv3 support. Here's how to fix the HTTP 401 errors."
id: poodle
tags: Stripe, Programming
topic: Stripe
description: "Here are three solutions to fixing HTTP 401 errors caused by Stripe removing SSLv3 support."
show_upsell: true
canonical_url: 'https://www.masteringmodernpayments.com/blog/stripe-ruby-sslv3-deprecated'
---

On November 15th [Stripe deprecated SSLv3](https://stripe.com/blog/poodle) because of the POODLE vulnerability. On the whole, this has been a good and welcome change, because SSLv3 has been terrible for a very long time.

The problem is that on some systems this causes backend API requests to start failing with an error message from Stripe because they're unable to auto-negotiate TLSv1.2.

--fold--

Here are three ways to help fix this:

## 1. Upgrade Ruby

This is the cleanest solution. Upgrade your Ruby to 2.1.4, 2.0.0-p594, or 1.9.3-p550. In those versions, [SSLv3 is disabled](https://www.ruby-lang.org/en/news/2014/10/27/changing-default-settings-of-ext-openssl/), which forces auto-negotiation to pick TLSv1.2.

You can also try upgrading your `rest-client` gem to the latest version in your `Gemfile`:

```ruby
gem 'rest-client', '>= 1.7.2'
```

This has worked for some people but it depends on your situation. You might already be running the latest, in which case carry on to the next option.

Also, if you're using the `rest_client` gem (notice the underscore instead of dash), be aware that this **re-enables SSLv3**. See [this commit](https://github.com/treeder/rest_client/commit/ae72fa75555ae8a20b51402461fb3adeb6bb3f50) from late October 2014.

## 2. Patch OpenSSL

At the bottom of the link in #1 there's a monkeypatch you can apply that changes OpenSSL to remove SSLv3.

## 3. Patch Stripe

If you can't or won't upgrade your Ruby and changing OpenSSL is too scary, you're left with the option of monkeypatching Stripe's library directly. Drop this code in an initializer:

```ruby
module Stripe
  def self.execute_request(opts)
    RestClient::Request.execute(opts.merge(ssl_version: :TLSv1))
  end
end
```

This is basically the solution that was [proposed to Stripe](https://github.com/stripe/stripe-ruby/pull/107) but they rejected it because when new versions of TLS come out it'll break. So, don't be surprised when it breaks two years down the line, but for now it works.

Of these three options, if you can go with the first one please do. It's the cleanest and least brittle solution.

**P.S.** Want me to solve these kinds of problems for your business, before they become problems? Contact me about [Payola Pro](https://www.payola.io/pro). You'll be glad you did.
