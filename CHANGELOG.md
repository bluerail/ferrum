## [0.4.0] - (Sep 17, 2019) ##

### Added

- `Ferrum::Network` module - moved logic from `Ferrum::Page::Net` with addition changes

- `Ferrum::Browser#network` - instance of `Ferrum::Network` from delegated `Ferrum::Page` instance

- `Ferrum::Network#request` & `Ferrum::Network#response` - delegation to `Network::Exchange` instance

- `Ferrum::Network#first_by` / `Ferrum::Network#last_by` - implemented searching by passed request_id in `Network::Exchange` instance 

- `Ferrum::Browser#traffic` delegation to `Ferrum::Network` of `Network::Exchange` instances

- `Ferrum::Network::Exchange` - simple request/response constructor with monitoring

    - `#build_request` - instance of `Network::Request` with passed params
    
    - `#build_response` - instance of `Network::Response` with passed params

    - `#build_error` - instance of `Network::Error` with passed params
    
    - `#navigation_request?` - the request verification on document by passed frame_id
    
    - `#blocked?` - boolean which becomes true when no the constructed response

    - `#to_a` - returns array of constructed request/response/error instances

- `Ferrum::Network::Request#type` - delegation to `type` of passed to instance params

- `Ferrum::Network::Request#type?` - boolean compare with `type` of instance with passed type as argument

- `Ferrum::Network::Request#frame_id` - delegation to `frameId` of passed to instance params

- `Ferrum::Network::InterceptedRequest#abort` - fires `continue` method of instance with `errorReason` as `Aborted`   

- `Ferrum::Network::InterceptedRequest#inspect` - simple implementation of native `inspect` method with returns of the current internal state  

- `Ferrum::Page::Frame#frame_id` - reader to public available of `frameId` by `Ferrum::Page#frame_id`

### Changed

- description of `Network/Authorization/Interception` sections in README

- `Ferrum::Browser#screenshot` & `Ferrum::Browser#pdf` methods are returns bin when no path is given

- `Ferrum::Browser#status` delegated to `Ferrum::Network`

- `Ferrum::Browser#authorize` delegated to `Ferrum::Network`

- `Ferrum::Network` module into `class` approach for `InterceptedRequest/Request/Response/Error` classes

- `Ferrum::Browser#intercept_request` into `Ferrum::Network#intercept`

- `Ferrum::Browser#subscribe` into `Ferrum::Network#subscribe` with public available

- `Ferrum::Browser#authorized_response` into `Ferrum::Network#authorized_response` with public available

- `Ferrum::Browser#clear_memory_cache` & `Ferrum::Browser#clear_network_traffic`

    merged to `Ferrum::Network#clear` with addition of `traffic` clear by the argument as symbol type of `traffic/cache`

- `Ferrum::Network::Request#time` - use `wallTime` params fir time detection

- `body_size` attribute writer of `Ferrum::Network::Response` with reduce of size on headers_size

    to handle `encodedDataLength` when `Network.responseReceived` is not dispatched

### Removed

- `Ferrum::Network::Response#redirect_url`

- `Ferrum::Page::Net`

- `Ferrum::Browser#abort_request`

- `Ferrum::Browser#continue_request`

- `Ferrum::Browser#response_headers`

- `Ferrum::Browser#network_traffic`

- `Ferrum::Network::InterceptedRequest#is_navigation_request=` (attribute writer)

## [0.3.0] - (Sep 12, 2019) ##

### Added

- CI build by TravisCI for ruby versions: `2.3/2.4/2.5/2.6/jruby-9.2.8.0`

- fix specs with support of MacOS time formats

- `Ferrum::Mouse::CLICK_WAIT` as `FERRUM_CLICK_WAIT` `ENV-var` with `0.1` as default value

- `Ferrum::Browser#authorize` option `:type` with valid values `:server` (by default), `:proxy`

- Logo :tada:

- `Ferrum::Node#inner_text` - evaluates JS: `this.innerText` on Node instance

- `Ferrum::Page::Runtime::INTERMITTENT_ATTEMPTS` as `FERRUM_INTERMITTENT_ATTEMPTS` `ENV-var` with `6` as default value

- `Ferrum::Page::Runtime::INTERMITTENT_SLEEP` as `FERRUM_INTERMITTENT_SLEEP` `ENV-var` with `0.1` as default value

- `Ferrum::Page#on` getting the `name` as option with `:dialog/:request_intercepted` cases & `block` as last argument

- `Ferrum::Browser#on` - delegated actions to `Ferrum::Page` instance

- `Ferrum::Dialog` object to handle JavaScript Dialog's

    - required `page, params` as init arguments

    - `#accept` fires JS: `Page.handleJavaScriptDialog` as command on provided `Ferrum::Page` instance with options which included `accept: true`

    - `#dismiss` fires JS: `Page.handleJavaScriptDialog` as command on provided `Ferrum::Page` instance with `accept: false`

    - `#match?` compare message by passed regexp

    - description of `Dialog` feature in README

- `Ferrum::Page::Event` extend of `Concurrent::Event` with implementation of `reset/wait` fix
 
    - implement `Ferrum::Page::Event#iteration` to reuse `synchronize` block on `@iteration` value of `Concurrent::Event`

    - redefinition of `Concurrent::Event#reset` - increase `@iteration` outside of `if @set` block

- `FERRUM_PROCESS_TIMEOUT` `ENV-var` as `Ferrum::Browser::Process::PROCESS_TIMEOUT` with `2` as default value

- Elapsed time implementation:

    - `Ferrum::Browser::Process::WAIT_KILLED` with `0.05`
    
    - `Ferrum.monotonic_time` - delegation to `Concurrent` object

    - `Ferrum.started` - class variable `@@started` as `monotonic_time`

    - `Ferrum.elapsed_time` - a difference of `monotonic_time` as minuend and passed time as argument or `@@started` as subtrahend

    - `Ferrum.timeout?` - boolean compare passed values `(start, timeout)` by `elapsed_time`

- JRuby support by replaces of `::Process::CLOCK_MONOTONIC` usages according to `Elapsed-time` implementation

### Changed

- fix globally changing of Thread behaviour on options `abort_on_exception/report_on_exception`

- `Ferrum::Page::Input#find_position` into `Ferrum::Node#find_position`

- `Ferrum::Browser#scroll_to` into `Ferrum::Mouse#scroll_to`

- option `:timeout` into `:wait` for `Ferrum::Page#command` / `Ferrum::Mouse#click`

- description of `Authorization` options in README

- `Ferrum::Page::Net#intercept_request` block as last argument into `Ferrum::Page::Net#on(:request_intercepted)` with passed block

- `Ferrum::Browser::TIMEOUT` into `Ferrum::Browser::DEFAULT_TIMEOUT` as `FERRUM_DEFAULT_TIMEOUT` `ENV-var` with `5` as default value

- usage of `Concurrent::Event` into `Ferrum::Page::Event` as `@event` of `Ferrum::Page` instance
 
- `Ferrum::Page::NEW_WINDOW_BUG_SLEEP` into `Ferrum::Page::NEW_WINDOW_WAIT` as `FERRUM_NEW_WINDOW_WAIT` `ENV-var` with `0.3` as default value 

### Removed

- `Ferrum::Page::Input`

- `Ferrum::Browser#proxy_authorize` / `Ferrum::Page::Net#proxy_authorize`

- `Ferrum::ModalNotFoundError`

- `Ferrum::Page#reset_modals` with delegation to `Ferrum::Browser`

- `Ferrum::Page#find_modal` with delegation to `Ferrum::Browser`

- `Ferrum::Page#accept_prompt` with delegation to `Ferrum::Browser`

- `Ferrum::Page#dismiss_confirm` with delegation to `Ferrum::Browser`

- `Ferrum::Page#accept_confirm` with delegation to `Ferrum::Browser`

- `Ferrum::Browser#on_request_intercepted`

## [0.2.1] - (Sep 5, 2019) ##

### Added

- handle `EOFError/Errno::ECONNRESET/Errno::EPIPE` errors with rescue

- description options of `Customization` in README

### Changed

- increased `Browser::Process::PROCESS_TIMEOUT` constant by 1

- `Ferrum::Network::InterceptedRequest#match?` to handle cases for `Ruby 2.3` and less
 
## [0.2.0] - (Sep 3, 2019) ##

### Added

- snippet examples of the actions in README

- `Ferrum::Node#focus` - fires the `command` `DOM.focus` on `Ferrum::Page` instance

- `Ferrum::Node#blur` - evaluates JS: `this.blur()` on `Ferrum::Page` instance

- `Ferrum::Node#click` - fires the native `click` on `Ferrum::Page` instance

- usage of `FERRUM_INTERMITTENT_ATTEMPTS` `ENV-var` on the rescue of runtime intermittent error

- implementation's of `Ferrum::Page::DOM#xpath` & `Ferrum::Page::DOM#at_xpath` 

- `Ferrum.with_attempts` - retry attempt with the sleep on the block passed as an argument

- `Ferrum::NoExecutionContextError` - raises when there's no `context` available

- `Ferrum::Node#attribute` - evaluates JS: `this.getAttribute` with passed `name`

- `Ferrum::Mouse` - dedicated class of `mouse` actions: `click/down/up/move`

- `Ferrum::Browser#mouse` - delegated actions to `Ferrum::Mouse` instance extracted from `Ferrum::Page::Input`

- `Ferrum::Page::Input#find_position` - usage of `DOM.getContentQuads` to find position of node by `top/left`

- `Ferrum::Keyboard` - dedicated class of `keyboard` actions: `down/up/type/modifiers`

- `Ferrum::Browser#keyboard` - delegated actions to `Ferrum::Keyboard` instance extracted from `Ferrum::Page::Input`

- `Ferrum::Headers` dedicated class of headers manager with `get/set/clear/add` actions which delegated to `Ferrum::Page` instance

- `Ferrum::Cookies` dedicated class which includes logic from `Ferrum::Browser::API::Cookie` & `Ferrum::Cookie` with actions: `all/[]/set/remove/clear`

- `Ferrum::Page#cookies` - delegated actions to `Ferrum::Cookies` instance

- `Ferrum::Page::Screenshot` module with methods `screenshot/pdf` implemented by commands `Page.captureScreenshot/Page.printToPDF`

- `Ferrum::Browser#screenshot` - delegated actions to `Page::Screenshot` module

- `Ferrum::Network::InterceptedRequest` class with methods: `auth_challenge?/match?/abort/continue/url/method/headers/initial_priority/referrer_policy`

- `Ferrum::Browser#intercept_request` - method with delegated to `Ferrum::Page::Net` which sets pattern into `Network.setRequestInterception`

- `Ferrum::Browser#on_request_intercepted` - method with delegated to `Ferrum::Page::Net` which applies passed block

- `Ferrum::Browser#abort_request` - method with delegated to `Ferrum::Page::Net` which stops request by passed interception_id

### Changed

- `Ferrum::Page::Input#send_keys` into `Ferrum::Page::Input#type`

- `Ferrum::DeadBrowser` into `Ferrum::DeadBrowserError`

- `Ferrum::ModalNotFound` into `Ferrum::ModalNotFoundError`

- `Ferrum::StatusFailError` into `Ferrum::StatusError`

- `Ferrum::NodeError` into `Ferrum::NodeNotFoundError`

- `Ferrum::Page#go_back` into `Ferrum::Page#back`

- `Ferrum::Page#go_forward` into `Ferrum::Page#forward`

- `Ferrum::Page::Dom#property` into `Ferrum::Page#property`

- `Ferrum::Page::Dom#select_file` into `Ferrum::Page#select_file`

- `Ferrum::Node::#click` getting the `mode` argument as option with `right/double/left` cases

- `Ferrum::Page::Frame#switch_to_frame` into `Ferrum::Page::Frame#within_frame` with added case of `ArgumentError`

### Removed

- `Ferrum::ObsoleteNode` error

- `Ferrum::FrameNotFound` error

- `Ferrum::Page::Input#set`

- `Ferrum::Page::Input#select`

- `Ferrum::Node::#attributes`

- `Ferrum::Node::#[]`

- `Ferrum::Node::#select_option`

- `Ferrum::Node::#unselect_option`

- `Ferrum::Node::#visible?`

- `Ferrum::Node::#checked?`

- `Ferrum::Node::#selected?`

- `Ferrum::Node::#disabled?`

- `Ferrum::Node::#path`

- `Ferrum::Node::#right_click`

- `Ferrum::Node::#double_click`

- `Ferrum::Page::Input#type`

- `Ferrum::Page::Input#generate_modifiers`

- `Ferrum::Browser::API` - `Header, Cookie, Screenshot, Intercept`

- `Ferrum::Browser#set_overrides`

- `Ferrum::Browser#url_whitelist`

- `Ferrum::Browser#url_blacklist`

## [0.1.2] - (Aug 27, 2019) ##

### Added

- catch of the intermittent errors inside of `evaluate's` methods

- `Ferrum::Page::Runtime#evaluate_on` - fires `Runtime.callFunctionOn` command with `functionDeclaration` on `Ferrum::Page`

### Removed

- `Ferrum::Page::Runtime#evaluate_in`

## [0.1.1] - (Aug 26, 2019) ##

### Added

- stringify the `url` which passed to `Ferrum::Page#goto`

## [0.1.0] - (Aug 26, 2019) ##

### Added

- fires the `Ferrum::NodeError` on zero of `node_id`

### Changed

- basic description in README 

## [0.1.0.alpha] - (Aug 2, 2019) ##

### Added

- Initial implementation

    #### Modules:

    - `Ferrum`

    - `Ferrum::Network` - simple requests/responses data store

    #### Classes:

    - `Ferrum::Browser` - basic command interface

    - `Ferrum::Cookie` - simple store of the cookie attributes

    - `Ferrum::Node` - abstract level of DOM-node with basic methods

    - `Ferrum::Page` - basic object of the command references, which included `DOM`, network and browser logic

    - `Ferrum::Targets` - initialize of the `window` manager with a clean browser state

    - classes of errors with a description of specific raises reasons

[0.4.0]: https://github.com/rubycdp/ferrum/compare/v0.3...v0.4
[0.3.0]: https://github.com/rubycdp/ferrum/compare/v0.2.1...v0.3
[0.2.1]: https://github.com/rubycdp/ferrum/compare/v0.2...v0.2.1
[0.2.0]: https://github.com/rubycdp/ferrum/compare/v0.1.2...v0.2
[0.1.2]: https://github.com/rubycdp/ferrum/compare/v0.1.1...v0.1.2
[0.1.1]: https://github.com/rubycdp/ferrum/compare/v0.1.0...v0.1.1
[0.1.0]: https://github.com/rubycdp/ferrum/compare/v0.1.0.alpha...v0.1.0
[0.1.0.alpha]: https://github.com/rubycdp/ferrum/releases/tag/v0.1.0.alpha
