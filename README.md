# 4DSphere API Documentation

API documentation for https://4dsphere.com
powered by Slate https://github.com/lord/slate

### Development Environment Setup
```
brew Install rbenv 

rbenv install 2.3.1

cd <project-director>

bundle install
```

### Deploy
```
bundle exec middleman build

aws s3 sync build s3://docs.4dsphere.com --acl public-read --cache-control "public, max-age=86400"
```

### Live Site
http://docs.4dsphere.com