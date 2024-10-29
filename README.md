## Bug Description

When adding those dependencies in the same project

- `org.springframework.boot:spring-boot-starter-oauth2-client`
- `org.springframework.cloud:spring-cloud-starter-gateway`

any filter that changes request header like `AddRequestHeader` will not work and throw this Exception

    java.lang.UnsupportedOperationException: null
    at org.springframework.http.ReadOnlyHttpHeaders.add(ReadOnlyHttpHeaders.java:95) ~[spring-web-6.1.14.jar:6.1.14]
    Suppressed: reactor.core.publisher.FluxOnAssembly$OnAssemblyException:
    Assembly trace from producer [reactor.core.publisher.MonoDefer] :
    reactor.core.publisher.Mono.defer(Mono.java:219)
    org.springframework.cloud.gateway.handler.FilteringWebHandler$DefaultGatewayFilterChain.filter(FilteringWebHandler.java:123)
    Error has been observed at the following site(s):
    *__________Mono.defer ⇢ at org.springframework.cloud.gateway.handler.FilteringWebHandler$DefaultGatewayFilterChain.filter(FilteringWebHandler.java:123)
    *__________Mono.defer ⇢ at org.springframework.cloud.gateway.handler.FilteringWebHandler$DefaultGatewayFilterChain.filter(FilteringWebHandler.java:123)
    *___________Mono.then ⇢ at org.springframework.cloud.gateway.filter.NettyWriteResponseFilter.filter(NettyWriteResponseFilter.java:69)
    |_    Mono.doOnCancel ⇢ at org.springframework.cloud.gateway.filter.NettyWriteResponseFilter.filter(NettyWriteResponseFilter.java:101)
    |_     Mono.doOnError ⇢ at org.springframework.cloud.gateway.filter.NettyWriteResponseFilter.filter(NettyWriteResponseFilter.java:102)
    *__________Mono.defer ⇢ at org.springframework.cloud.gateway.handler.FilteringWebHandler$DefaultGatewayFilterChain.filter(FilteringWebHandler.java:123)
    *__________Mono.defer ⇢ at org.springframework.cloud.gateway.handler.FilteringWebHandler$DefaultGatewayFilterChain.filter(FilteringWebHandler.java:123)
    |_     Mono.doFinally ⇢ at org.springframework.cloud.gateway.filter.RemoveCachedBodyFilter.filter(RemoveCachedBodyFilter.java:35)
    *__________Mono.defer ⇢ at org.springframework.cloud.gateway.handler.FilteringWebHandler$DefaultGatewayFilterChain.filter(FilteringWebHandler.java:123)
    *___________Mono.then ⇢ at org.springframework.web.reactive.result.SimpleHandlerAdapter.handle(SimpleHandlerAdapter.java:46)
    |_ Mono.onErrorResume ⇢ at org.springframework.web.reactive.DispatcherHandler.handleResultMono(DispatcherHandler.java:168)
    |_       Mono.flatMap ⇢ at org.springframework.web.reactive.DispatcherHandler.handleResultMono(DispatcherHandler.java:172)
    *__________Mono.error ⇢ at org.springframework.web.reactive.result.method.annotation.RequestMappingHandlerAdapter.handleException(RequestMappingHandlerAdapter.java:317)
    *________Mono.flatMap ⇢ at org.springframework.web.reactive.DispatcherHandler.handle(DispatcherHandler.java:154)
    *__________Mono.defer ⇢ at org.springframework.web.server.handler.DefaultWebFilterChain.filter(DefaultWebFilterChain.java:106)
    |_         checkpoint ⇢ org.springframework.cloud.gateway.filter.WeightCalculatorWebFilter [DefaultWebFilterChain]
    *__________Mono.defer ⇢ at org.springframework.web.server.handler.DefaultWebFilterChain.filter(DefaultWebFilterChain.java:106)
    *__________Mono.defer ⇢ at org.springframework.web.server.handler.DefaultWebFilterChain.filter(DefaultWebFilterChain.java:106)
    *__Mono.switchIfEmpty ⇢ at org.springframework.security.web.server.authorization.AuthorizationWebFilter.filter(AuthorizationWebFilter.java:56)
    |_         checkpoint ⇢ org.springframework.security.web.server.authorization.AuthorizationWebFilter [DefaultWebFilterChain]
    *__________Mono.defer ⇢ at org.springframework.web.server.handler.DefaultWebFilterChain.filter(DefaultWebFilterChain.java:106)
    |_ Mono.onErrorResume ⇢ at org.springframework.security.web.server.authorization.ExceptionTranslationWebFilter.filter(ExceptionTranslationWebFilter.java:53)
    |_         checkpoint ⇢ org.springframework.security.web.server.authorization.ExceptionTranslationWebFilter [DefaultWebFilterChain]
    *__________Mono.defer ⇢ at org.springframework.web.server.handler.DefaultWebFilterChain.filter(DefaultWebFilterChain.java:106)
    *___________Mono.then ⇢ at org.springframework.security.web.server.authentication.logout.LogoutWebFilter.filter(LogoutWebFilter.java:63)
    *__Mono.switchIfEmpty ⇢ at org.springframework.security.web.server.authentication.logout.LogoutWebFilter.filter(LogoutWebFilter.java:63)
    |_           Mono.map ⇢ at org.springframework.security.web.server.authentication.logout.LogoutWebFilter.filter(LogoutWebFilter.java:64)
    |_       Mono.flatMap ⇢ at org.springframework.security.web.server.authentication.logout.LogoutWebFilter.filter(LogoutWebFilter.java:65)
    |_       Mono.flatMap ⇢ at org.springframework.security.web.server.authentication.logout.LogoutWebFilter.filter(LogoutWebFilter.java:66)
    |_         checkpoint ⇢ org.springframework.security.web.server.authentication.logout.LogoutWebFilter [DefaultWebFilterChain]
    *__________Mono.defer ⇢ at org.springframework.web.server.handler.DefaultWebFilterChain.filter(DefaultWebFilterChain.java:106)
    *________Mono.flatMap ⇢ at org.springframework.security.web.server.savedrequest.ServerRequestCacheWebFilter.filter(ServerRequestCacheWebFilter.java:41)
    |_         checkpoint ⇢ org.springframework.security.web.server.savedrequest.ServerRequestCacheWebFilter [DefaultWebFilterChain]
    *__________Mono.defer ⇢ at org.springframework.web.server.handler.DefaultWebFilterChain.filter(DefaultWebFilterChain.java:106)
    |_         checkpoint ⇢ org.springframework.security.web.server.context.SecurityContextServerWebExchangeWebFilter [DefaultWebFilterChain]
    *__________Mono.defer ⇢ at org.springframework.web.server.handler.DefaultWebFilterChain.filter(DefaultWebFilterChain.java:106)
    |_  Mono.contextWrite ⇢ at org.springframework.security.web.server.context.ReactorContextWebFilter.filter(ReactorContextWebFilter.java:48)
    |_         checkpoint ⇢ org.springframework.security.web.server.context.ReactorContextWebFilter [DefaultWebFilterChain]
    *__________Mono.defer ⇢ at org.springframework.web.server.handler.DefaultWebFilterChain.filter(DefaultWebFilterChain.java:106)
    *__________Mono.defer ⇢ at org.springframework.security.web.server.csrf.CsrfWebFilter.continueFilterChain(CsrfWebFilter.java:148)
    *___________Mono.then ⇢ at org.springframework.security.web.server.csrf.CsrfWebFilter.filter(CsrfWebFilter.java:126)
    *__Mono.switchIfEmpty ⇢ at org.springframework.security.web.server.csrf.CsrfWebFilter.filter(CsrfWebFilter.java:126)
    |_ Mono.onErrorResume ⇢ at org.springframework.security.web.server.csrf.CsrfWebFilter.filter(CsrfWebFilter.java:127)
    |_         checkpoint ⇢ org.springframework.security.web.server.csrf.CsrfWebFilter [DefaultWebFilterChain]
    *__________Mono.defer ⇢ at org.springframework.web.server.handler.DefaultWebFilterChain.filter(DefaultWebFilterChain.java:106)
    |_         checkpoint ⇢ org.springframework.security.web.server.header.HttpHeaderWriterWebFilter [DefaultWebFilterChain]
    *__________Mono.defer ⇢ at org.springframework.web.server.handler.DefaultWebFilterChain.filter(DefaultWebFilterChain.java:106)
    |_  Mono.contextWrite ⇢ at org.springframework.security.config.web.server.ServerHttpSecurity$ServerWebExchangeReactorContextWebFilter.filter(ServerHttpSecurity.java:3968)
    |_         checkpoint ⇢ org.springframework.security.config.web.server.ServerHttpSecurity$ServerWebExchangeReactorContextWebFilter [DefaultWebFilterChain]
    *__________Mono.defer ⇢ at org.springframework.web.server.handler.DefaultWebFilterChain.filter(DefaultWebFilterChain.java:106)
    *________Mono.flatMap ⇢ at org.springframework.security.web.server.WebFilterChainProxy.filterFirewalledExchange(WebFilterChainProxy.java:78)
    *________Mono.flatMap ⇢ at org.springframework.security.web.server.WebFilterChainProxy.filter(WebFilterChainProxy.java:65)
    |_ Mono.onErrorResume ⇢ at org.springframework.security.web.server.WebFilterChainProxy.filter(WebFilterChainProxy.java:66)
    |_         checkpoint ⇢ org.springframework.security.web.server.WebFilterChainProxy [DefaultWebFilterChain]
    *__________Mono.defer ⇢ at org.springframework.web.server.handler.DefaultWebFilterChain.filter(DefaultWebFilterChain.java:106)
    |_     Mono.doOnError ⇢ at org.springframework.web.server.handler.ExceptionHandlingWebHandler.handle(ExceptionHandlingWebHandler.java:84)
    |_ Mono.onErrorResume ⇢ at org.springframework.web.server.handler.ExceptionHandlingWebHandler.handle(ExceptionHandlingWebHandler.java:85)
    |_     Mono.doOnError ⇢ at org.springframework.web.server.handler.ExceptionHandlingWebHandler.handle(ExceptionHandlingWebHandler.java:84)
    *__________Mono.error ⇢ at org.springframework.web.server.handler.ExceptionHandlingWebHandler$CheckpointInsertingHandler.handle(ExceptionHandlingWebHandler.java:106)
    |_         checkpoint ⇢ HTTP GET "/example" [ExceptionHandlingWebHandler]
    Original Stack Trace:
    at org.springframework.http.ReadOnlyHttpHeaders.add(ReadOnlyHttpHeaders.java:95) ~[spring-web-6.1.14.jar:6.1.14]
    at org.springframework.http.ReadOnlyHttpHeaders.add(ReadOnlyHttpHeaders.java:39) ~[spring-web-6.1.14.jar:6.1.14]
    at org.springframework.http.HttpHeaders.add(HttpHeaders.java:1712) ~[spring-web-6.1.14.jar:6.1.14]
    at org.springframework.cloud.gateway.filter.factory.AddRequestHeaderGatewayFilterFactory$1.lambda$filter$0(AddRequestHeaderGatewayFilterFactory.java:41) ~[spring-cloud-gateway-server-4.1.5.jar:4.1.5]
    at org.springframework.http.server.reactive.DefaultServerHttpRequestBuilder.headers(DefaultServerHttpRequestBuilder.java:117) ~[spring-web-6.1.14.jar:6.1.14]
    at org.springframework.cloud.gateway.filter.factory.AddRequestHeaderGatewayFilterFactory$1.filter(AddRequestHeaderGatewayFilterFactory.java:41) ~[spring-cloud-gateway-server-4.1.5.jar:4.1.5]
    at org.springframework.cloud.gateway.filter.OrderedGatewayFilter.filter(OrderedGatewayFilter.java:44) ~[spring-cloud-gateway-server-4.1.5.jar:4.1.5]
    at org.springframework.cloud.gateway.handler.FilteringWebHandler$DefaultGatewayFilterChain.lambda$filter$0(FilteringWebHandler.java:127) ~[spring-cloud-gateway-server-4.1.5.jar:4.1.5]
    at reactor.core.publisher.MonoDefer.subscribe(MonoDefer.java:45) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.InternalMonoOperator.subscribe(InternalMonoOperator.java:76) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoDefer.subscribe(MonoDefer.java:53) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Mono.subscribe(Mono.java:4576) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.subscribeNext(MonoIgnoreThen.java:265) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoIgnoreThen.subscribe(MonoIgnoreThen.java:51) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.InternalMonoOperator.subscribe(InternalMonoOperator.java:76) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoDefer.subscribe(MonoDefer.java:53) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.InternalMonoOperator.subscribe(InternalMonoOperator.java:76) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoDefer.subscribe(MonoDefer.java:53) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.InternalMonoOperator.subscribe(InternalMonoOperator.java:76) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoDefer.subscribe(MonoDefer.java:53) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Mono.subscribe(Mono.java:4576) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.subscribeNext(MonoIgnoreThen.java:265) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoIgnoreThen.subscribe(MonoIgnoreThen.java:51) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.InternalMonoOperator.subscribe(InternalMonoOperator.java:76) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoFlatMap$FlatMapMain.onNext(MonoFlatMap.java:165) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxOnErrorResume$ResumeSubscriber.onNext(FluxOnErrorResume.java:79) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxSwitchIfEmpty$SwitchIfEmptySubscriber.onNext(FluxSwitchIfEmpty.java:74) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoNext$NextSubscriber.onNext(MonoNext.java:82) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxConcatMapNoPrefetch$FluxConcatMapNoPrefetchSubscriber.innerNext(FluxConcatMapNoPrefetch.java:259) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxConcatMap$ConcatMapInner.onNext(FluxConcatMap.java:865) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxMapFuseable$MapFuseableSubscriber.onNext(FluxMapFuseable.java:129) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxSwitchIfEmpty$SwitchIfEmptySubscriber.onNext(FluxSwitchIfEmpty.java:74) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxMapFuseable$MapFuseableSubscriber.onNext(FluxMapFuseable.java:129) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxMapFuseable$MapFuseableSubscriber.onNext(FluxMapFuseable.java:129) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoNext$NextSubscriber.onNext(MonoNext.java:82) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxConcatMapNoPrefetch$FluxConcatMapNoPrefetchSubscriber.innerNext(FluxConcatMapNoPrefetch.java:259) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxConcatMap$ConcatMapInner.onNext(FluxConcatMap.java:865) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxOnErrorResume$ResumeSubscriber.onNext(FluxOnErrorResume.java:79) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoPeekTerminal$MonoTerminalPeekSubscriber.onNext(MonoPeekTerminal.java:180) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoFilterWhen$MonoFilterWhenMain.onNext(MonoFilterWhen.java:136) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$ScalarSubscription.request(Operators.java:2571) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoFilterWhen$MonoFilterWhenMain.request(MonoFilterWhen.java:183) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoPeekTerminal$MonoTerminalPeekSubscriber.request(MonoPeekTerminal.java:139) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.request(Operators.java:2331) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.request(Operators.java:2331) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxConcatMapNoPrefetch$FluxConcatMapNoPrefetchSubscriber.request(FluxConcatMapNoPrefetch.java:339) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoNext$NextSubscriber.request(MonoNext.java:108) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxMapFuseable$MapFuseableSubscriber.request(FluxMapFuseable.java:171) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxMapFuseable$MapFuseableSubscriber.request(FluxMapFuseable.java:171) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.request(Operators.java:2331) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxMapFuseable$MapFuseableSubscriber.request(FluxMapFuseable.java:171) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.request(Operators.java:2331) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxConcatMapNoPrefetch$FluxConcatMapNoPrefetchSubscriber.request(FluxConcatMapNoPrefetch.java:339) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoNext$NextSubscriber.request(MonoNext.java:108) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.set(Operators.java:2367) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.onSubscribe(Operators.java:2241) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoNext$NextSubscriber.onSubscribe(MonoNext.java:70) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxConcatMapNoPrefetch$FluxConcatMapNoPrefetchSubscriber.onSubscribe(FluxConcatMapNoPrefetch.java:164) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxIterable.subscribe(FluxIterable.java:201) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxIterable.subscribe(FluxIterable.java:83) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.InternalMonoOperator.subscribe(InternalMonoOperator.java:76) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoDefer.subscribe(MonoDefer.java:53) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.InternalMonoOperator.subscribe(InternalMonoOperator.java:76) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoDefer.subscribe(MonoDefer.java:53) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.InternalMonoOperator.subscribe(InternalMonoOperator.java:76) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoDefer.subscribe(MonoDefer.java:53) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Mono.subscribe(Mono.java:4576) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxSwitchIfEmpty$SwitchIfEmptySubscriber.onComplete(FluxSwitchIfEmpty.java:82) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoPeekTerminal$MonoTerminalPeekSubscriber.onComplete(MonoPeekTerminal.java:299) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoPeekTerminal$MonoTerminalPeekSubscriber.onComplete(MonoPeekTerminal.java:299) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoFlatMap$FlatMapMain.onNext(MonoFlatMap.java:155) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxSwitchIfEmpty$SwitchIfEmptySubscriber.onNext(FluxSwitchIfEmpty.java:74) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxFilterFuseable$FilterFuseableSubscriber.onNext(FluxFilterFuseable.java:118) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxDefaultIfEmpty$DefaultIfEmptySubscriber.onNext(FluxDefaultIfEmpty.java:122) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoNext$NextSubscriber.onNext(MonoNext.java:82) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxConcatMapNoPrefetch$FluxConcatMapNoPrefetchSubscriber.innerNext(FluxConcatMapNoPrefetch.java:259) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxConcatMap$ConcatMapInner.onNext(FluxConcatMap.java:865) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoFlatMap$FlatMapMain.onNext(MonoFlatMap.java:158) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxMapFuseable$MapFuseableSubscriber.onNext(FluxMapFuseable.java:129) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxFilter$FilterSubscriber.onNext(FluxFilter.java:113) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$ScalarSubscription.request(Operators.java:2571) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxFilter$FilterSubscriber.request(FluxFilter.java:186) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxMapFuseable$MapFuseableSubscriber.request(FluxMapFuseable.java:171) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoFlatMap$FlatMapMain.request(MonoFlatMap.java:194) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.request(Operators.java:2331) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxConcatMapNoPrefetch$FluxConcatMapNoPrefetchSubscriber.request(FluxConcatMapNoPrefetch.java:339) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoNext$NextSubscriber.request(MonoNext.java:108) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxDefaultIfEmpty$DefaultIfEmptySubscriber.request(FluxDefaultIfEmpty.java:98) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxFilterFuseable$FilterFuseableSubscriber.request(FluxFilterFuseable.java:191) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.set(Operators.java:2367) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.onSubscribe(Operators.java:2241) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxFilterFuseable$FilterFuseableSubscriber.onSubscribe(FluxFilterFuseable.java:87) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$BaseFluxToMonoOperator.onSubscribe(Operators.java:2051) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoNext$NextSubscriber.onSubscribe(MonoNext.java:70) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxConcatMapNoPrefetch$FluxConcatMapNoPrefetchSubscriber.onSubscribe(FluxConcatMapNoPrefetch.java:164) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxIterable.subscribe(FluxIterable.java:201) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxIterable.subscribe(FluxIterable.java:83) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.InternalMonoOperator.subscribe(InternalMonoOperator.java:76) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoDefer.subscribe(MonoDefer.java:53) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.InternalMonoOperator.subscribe(InternalMonoOperator.java:76) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoDefer.subscribe(MonoDefer.java:53) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Mono.subscribe(Mono.java:4576) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoIgnoreThen$ThenIgnoreMain.subscribeNext(MonoIgnoreThen.java:265) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoIgnoreThen.subscribe(MonoIgnoreThen.java:51) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Mono.subscribe(Mono.java:4576) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxSwitchIfEmpty$SwitchIfEmptySubscriber.onComplete(FluxSwitchIfEmpty.java:82) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxFilterFuseable$FilterFuseableSubscriber.onComplete(FluxFilterFuseable.java:171) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxPeekFuseable$PeekFuseableConditionalSubscriber.onComplete(FluxPeekFuseable.java:595) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxSwitchIfEmpty$SwitchIfEmptySubscriber.onComplete(FluxSwitchIfEmpty.java:85) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$ScalarSubscription.request(Operators.java:2573) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.set(Operators.java:2367) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$MultiSubscriptionSubscriber.onSubscribe(Operators.java:2241) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoJust.subscribe(MonoJust.java:55) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Mono.subscribe(Mono.java:4576) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxSwitchIfEmpty$SwitchIfEmptySubscriber.onComplete(FluxSwitchIfEmpty.java:82) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoNext$NextSubscriber.onComplete(MonoNext.java:102) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxFilterFuseable$FilterFuseableSubscriber.onComplete(FluxFilterFuseable.java:171) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxFlatMap$FlatMapMain.checkTerminated(FluxFlatMap.java:850) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxFlatMap$FlatMapMain.drainLoop(FluxFlatMap.java:612) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxFlatMap$FlatMapMain.drain(FluxFlatMap.java:592) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxFlatMap$FlatMapMain.onComplete(FluxFlatMap.java:469) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxPeekFuseable$PeekFuseableSubscriber.onComplete(FluxPeekFuseable.java:277) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxIterable$IterableSubscription.slowPath(FluxIterable.java:357) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxIterable$IterableSubscription.request(FluxIterable.java:294) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxPeekFuseable$PeekFuseableSubscriber.request(FluxPeekFuseable.java:144) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxFlatMap$FlatMapMain.onSubscribe(FluxFlatMap.java:373) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxPeekFuseable$PeekFuseableSubscriber.onSubscribe(FluxPeekFuseable.java:178) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxIterable.subscribe(FluxIterable.java:201) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxIterable.subscribe(FluxIterable.java:83) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.InternalMonoOperator.subscribe(InternalMonoOperator.java:76) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoDefer.subscribe(MonoDefer.java:53) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.InternalMonoOperator.subscribe(InternalMonoOperator.java:76) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoFlatMap$FlatMapMain.onNext(MonoFlatMap.java:165) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$BaseFluxToMonoOperator.completePossiblyEmpty(Operators.java:2097) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxDefaultIfEmpty$DefaultIfEmptySubscriber.onComplete(FluxDefaultIfEmpty.java:134) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxMapFuseable$MapFuseableSubscriber.onComplete(FluxMapFuseable.java:152) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxMapFuseable$MapFuseableSubscriber.onComplete(FluxMapFuseable.java:152) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxFilterFuseable$FilterFuseableSubscriber.onComplete(FluxFilterFuseable.java:171) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxMapFuseable$MapFuseableConditionalSubscriber.onComplete(FluxMapFuseable.java:350) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.Operators$MonoSubscriber.complete(Operators.java:1866) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoCacheTime$CoordinatorSubscriber.signalCached(MonoCacheTime.java:337) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoCacheTime$CoordinatorSubscriber.onNext(MonoCacheTime.java:354) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxPeekFuseable$PeekFuseableSubscriber.onNext(FluxPeekFuseable.java:210) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.FluxSwitchIfEmpty$SwitchIfEmptySubscriber.onNext(FluxSwitchIfEmpty.java:74) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoPeekTerminal$MonoTerminalPeekSubscriber.onNext(MonoPeekTerminal.java:180) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.publisher.MonoPublishOn$PublishOnSubscriber.run(MonoPublishOn.java:181) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:68) ~[reactor-core-3.6.11.jar:3.6.11]
    at reactor.core.scheduler.SchedulerTask.call(SchedulerTask.java:28) ~[reactor-core-3.6.11.jar:3.6.11]
    at java.base/java.util.concurrent.FutureTask.run$$$capture(FutureTask.java:264) ~[na:na]
    at java.base/java.util.concurrent.FutureTask.run(FutureTask.java) ~[na:na]
    at java.base/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:304) ~[na:na]
    at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1136) ~[na:na]
    at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:635) ~[na:na]
    at java.base/java.lang.Thread.run(Thread.java:840) ~[na:na]

### steps to produce it

- run the application using `gradle bootRun`
- access this endpoint `http://localhost:8080/example`