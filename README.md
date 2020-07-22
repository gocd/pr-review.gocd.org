# Pull request reviews for gocd website.

Open the URL https://gocd.github.io/pr-review.gocd.org/pr-$PR_NUMBER to see the results of your PR.

## Periodically pruning the repository

- ensure that you have the `gh` cli installed

```shell
# change to the directory containing this repo: https://github.com/gocd/www.go.cd
cd www.go.cd

# save the list of open pull requests in a variable
PRS=$(gh pr list | awk '{print $1}' | sed -e 's/#//g')

# clone this repo
cd /tmp
git clone https://github.com/gocd/pr-review.gocd.org
cd pr-review.gocd.org

# delete dirs we do not need anymore
for i in $(/bin/ls | sed -e s/pr-//g); do
  if echo $PRS | grep $i -q; then
    echo open $i;
  else
    rm -rf pr-$i
  fi
done

git add .
git commit -m 'prune'

#squash commits all the way
git rebase SECOND_SHA_IN_HISTORY -i

# force push
git push -f
```
