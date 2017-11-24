#Logger with winston (Server)

* logger.winston.js
```
/**
 * js logger with winston
 * @author Charly Lee
 * @description format style
 * @version 0.0.1
 */
var winston = require('winston');
require('winston-daily-rotate-file');
var dateFormat = require('dateformat');
var path = require('path');
var env_mode = process.env.ENV || "developement";

module.exports = function (process_nm) {
	if (!process_nm) process_nm = 'unknown';
	else process_nm = path.basename(process_nm, '.js');

	var timestamp = function () {
		return dateFormat(new Date(), "yyyy-mm-dd hh:MM:ss");
	}

	var formatter = function (options) {
		return options.timestamp() + ' ' + options.level.toUpperCase()[0] + ' [' + process_nm + '] ' + (options.message ? options.message : '') + (options.meta && Object.keys(options.meta).length ? JSON.stringify(options.meta) : '');
	}

	var transportConsole = new (winston.transports.Console)({
		level: 'debug', /* { error: 0, warn: 1, info: 2, verbose: 3, debug: 4, silly: 5 } */
		json: false, /* true : will log out multi-line JSON objects */
		stringify: false,
		timestamp: timestamp,
		formatter: formatter
	});

	var transportDailyRotateFile = new (winston.transports.DailyRotateFile)({
		filename: './modules-server/winston/log/log',
		datePattern: 'yyyy-MM-dd.',
		prepend: true,
		level: 'info',
		json: false,
		stringify: false,
		timestamp: timestamp,
		formatter: formatter
	});

	var transports = env_mode === "developement" ? [transportConsole] : [transportDailyRotateFile];

	return new (winston.Logger)({ transports: transports });
}
```

* logger.winston.test.js
```
var logger = require('./logger.winston')(__filename || "Process Name");

var appName = 'Test-Logger-Winston';
var env_mode = process.env.ENV || "developement";

logger.debug('application [%s] is starting in [%s] mode.');
logger.info('application [%s] is starting in [%s] mode.', appName, env_mode);
logger.warn('application [%s] is starting in [%s] mode.', appName, env_mode);
logger.error('application [%s] is starting in [%s] mode.', appName, env_mode);
logger.log('debug', 'application [%s] is starting in [%s] mode.', appName, env_mode);
logger.log('info', 'application [%s] is starting in [%s] mode.', appName, env_mode);
logger.log('warn', 'application [%s] is starting in [%s] mode.', appName, env_mode);
logger.log('error', 'application [%s] is starting in [%s] mode.', appName, env_mode);

var objSmaple = {"cookie":{"path":"/","_expires":null,"originalMaxAge":null,"httpOnly":true},"messages":[{"type":"message_error","content":"Sorry! invalid credentials."}]};

logger.debug('objSmaple=' + objSmaple);
logger.debug('objSmaple=%s', objSmaple);
logger.debug('objSmaple=%j', objSmaple);
```
* output
```
2017-11-24 02:18:28 [D] [logger.winston.test] application [%s] is starting in [%s] mode.
2017-11-24 02:18:28 [I] [logger.winston.test] application [Test-Logger-Winston] is starting in [developement] mode.
2017-11-24 02:18:28 [W] [logger.winston.test] application [Test-Logger-Winston] is starting in [developement] mode.
2017-11-24 02:18:28 [E] [logger.winston.test] application [Test-Logger-Winston] is starting in [developement] mode.
2017-11-24 02:18:28 [D] [logger.winston.test] application [Test-Logger-Winston] is starting in [developement] mode.
2017-11-24 02:18:28 [I] [logger.winston.test] application [Test-Logger-Winston] is starting in [developement] mode.
2017-11-24 02:18:28 [W] [logger.winston.test] application [Test-Logger-Winston] is starting in [developement] mode.
2017-11-24 02:18:28 [E] [logger.winston.test] application [Test-Logger-Winston] is starting in [developement] mode.
2017-11-24 02:18:28 [D] [logger.winston.test] objSmaple=[object Object]
2017-11-24 02:18:28 [D] [logger.winston.test] objSmaple=[object Object]
2017-11-24 02:18:28 [D] [logger.winston.test] objSmaple={"cookie":{"path":"/","_expires":null,"originalMaxAge":null,"httpOnly":true},"messages":[{"type":"message_error","content":"Sorry! invalid credentials."}]}
```