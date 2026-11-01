```
get socialNetwork() {
    return this.getAttribute('socialNetwork') || 'tw';
}

set socialNetwork(newValue) {
    this.setAttribute('socialNetwork', newValue);
}

get user() {
    return this.getAttribute('user') || 'none';
}

set user(newValue) {
    this.setAttribute('user', newValue);
}

static twTemplate(user) {
    return `
${SocialShareComponent.twStyle()}
<span class="twitter-button">
    <a href="https://twitter.com/${user}">
        Follow @${user}
    </a>
</span>`;
}

static twStyle() {
    return `
<style>
a {
    height: 20px;
    padding: 3px 6px;
    background-color: #1b95e0;
    color: #fff;
    border-radius: 3px;
    font-weight: 500;
    font-size: 11px;
    font-family:'Helvetica Neue', Arial, sans-serif;

```