import { createServer } from 'node:http';
import { createWriteStream, readFile } from 'node:fs';
import { URL } from 'url';

const log = createWriteStream('log.txt', { flags: 'a' });

const srv = createServer((req, res) => {
  const url = new URL(req.url, 'http://localhost');

  const send = (code, type, content) => {
    res.writeHead(code, { 'Content-Type': type });
    res.end(content);
  };

  switch (url.pathname) {
    case '/':
      send(200, 'text/html', '<h1 style="font-size:24px;font-weight:bold;">Selling banana, humans!</h1>');
      break;

    case '/user':
      send(200, 'text/html', '<h1 style="font-size:24px;font-weight:bold;">Joking not bananas</h1>');
      break;

    case '/favicon.ico':
      readFile("C:\\Users\\Dfg\\.vscode\\cli\\favicon.ico", (err, data) => {
        if (err) return send(500, 'text/plain', 'Error loading favicon.ico\n');
        send(200, 'image/x-icon', data);
      });
      break;

    default:
      send(404, 'text/html', '<h1 style="font-size:24px;font-weight:bold;">404 not found</h1>');
  }

  const ip = req.socket.remoteAddress;
  const ua = req.headers['user-agent'] || 'Unknown';
  log.write(`Time: ${new Date().toISOString()}, IP: ${ip}, UA: ${ua}\n`);
});

srv.listen(4000, '127.0.0.1', () => {
  console.log('Listening on 127.0.0.1:4000');
});
