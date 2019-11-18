# Travis Notes

### First Steps on Travis
* Follow this reference: https://docs.travis-ci.com/user/tutorial/

### Encrypt an env variable
* Reference: https://docs.travis-ci.com/user/environment-variables#encrypting-environment-variables

* Install travis
```bash
gem install travis
```
* In your repository directory, run:
```
travis encrypt DOCKER_USERNAME=jcabelloc --add env.global
travis encrypt DOCKER_PASSWORD=******************** --add env.global
travis encrypt AWS_ACCESS_KEY=******************** --add env.global
travis encrypt AWS_SECRET_KEY=******************** --add env.global
travis encrypt GITHUB_TOKEN=******************** --add env.global
```