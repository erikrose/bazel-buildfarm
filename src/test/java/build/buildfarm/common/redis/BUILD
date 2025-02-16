config_setting(
    name = "native-redis",
    values = {"define": "native-redis=true"},
)

COMMON_DEPS = [
    "//src/main/java/build/buildfarm/instance/shard",
    "//src/main/java/build/buildfarm/common",
    "//src/main/java/build/buildfarm/common/redis",
    "//src/main/protobuf:build_buildfarm_v1test_buildfarm_java_proto",
    "//src/test/java/build/buildfarm:test_runner",
    "//third_party/jedis",
    "@maven//:com_google_truth_truth",
    "@maven//:io_grpc_grpc_api",
    "@maven//:org_mockito_mockito_core",
]

NATIVE_REDIS_TESTS = [
    "BalancedRedisQueueTest.java",
    "RedisNodeHashesTest.java",
    "RedisQueueTest.java",
    "RedisPriorityQueueTest.java",
]

filegroup(
    name = "lua_resources",
    srcs = ["//src/main/java/build/buildfarm/common/redis:zpoplpush.lua"],
    visibility = ["//visibility:public"],
)

java_test(
    name = "tests",
    size = "small",
    srcs = glob(
        ["*.java"],
        exclude = NATIVE_REDIS_TESTS,
    ),
    classpath_resources = [":lua_resources"],
    tags = ["exclusive"],
    test_class = "build.buildfarm.AllTests",
    deps = COMMON_DEPS,
)

java_test(
    name = "priorityqueue",
    size = "small",
    srcs = select({
        ":native-redis": ["RedisPriorityQueueTest.java"],
        "//conditions:default": ["RedisPriorityQueueMockTest.java"],
    }),
    classpath_resources = [":lua_resources"],
    tags = ["exclusive"],
    test_class = "build.buildfarm.AllTests",
    deps = COMMON_DEPS,
)

java_test(
    name = "queue",
    size = "small",
    srcs = select({
        ":native-redis": ["RedisQueueTest.java"],
        "//conditions:default": ["RedisQueueMockTest.java"],
    }),
    tags = ["exclusive"],
    test_class = "build.buildfarm.AllTests",
    deps = COMMON_DEPS,
)

java_test(
    name = "balancedqueue",
    size = "small",
    srcs = select({
        ":native-redis": ["BalancedRedisQueueTest.java"],
        "//conditions:default": ["BalancedRedisQueueMockTest.java"],
    }),
    classpath_resources = [":lua_resources"],
    tags = ["exclusive"],
    test_class = "build.buildfarm.AllTests",
    deps = COMMON_DEPS,
)
