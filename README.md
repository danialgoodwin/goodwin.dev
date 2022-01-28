# goodwin.dev
My personal site is made with Orchid and Netlify (including [Netlify CMS](https://www.netlifycms.org/)).

## Workflow

### Setup
```bash
mkdir -p ~/a/repo
cd ~/a/repo
git clone git@github.com:danialgoodwin/goodwin.dev.git
cd goodwin.dev
./gradlew orchidServe

# View the generated site at http://localhost:8080/
```

### Development

    git commit -am "..."

### Testing

    yes

### Deployment

    git push


# TODO
- Maybe support offline documentation via PDF: https://orchid.run/plugins/orchidwiki#offline-documentation

TODO: Eventually add the following that currently scattered about
    - notes (links)
    - ideas
    - blog
    - cheat-sheets
    - pages (motivation, interests, projects, recipes?)
