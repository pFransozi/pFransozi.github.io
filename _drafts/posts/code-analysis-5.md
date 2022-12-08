# code snippts

## app.js

~~~ js
const app = require('express')();
const a = require('axios');
const d = require('dns');

function antiSSRF(req, res, next){
    const url = new URL(req.query.file);

    d.lookup(url.hostname, (err, addr) => {
        if (addr === '123.61.186.155') next();
        else res.send('Not allowed!');
    })
}

app.get('/copy_file', antiSSRF, (req, res) =>{
    setTimeout(function() {
        a.get(req.query.file).then((response) => {
            res.send('Done: ' + response.data);
        })

    }, 3000);
})

app.listen(3000, '0.0.0.0');
~~~

## local.js

~~~ js
const app = require('express')();

app.get('/flag', (req, res) => {
    res.send('flag{s4mpl3_fl4g}');
});

app.listen(3000, '127.0.0.1');
~~~

https://twitter.com/intigriti/status/1600837243308244992