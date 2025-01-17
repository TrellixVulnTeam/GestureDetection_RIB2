# Copyright 2019 The MediaPipe Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

licenses(["notice"])

load("@rules_java//java:defs.bzl", "java_binary", "java_lite_proto_library", "java_proto_library")

# Basic library common across example apps.
android_library(
    name = "handcontrol",
    srcs = glob(["*.java"]),
    assets = [
        "//mediapipe/examples/android/src/java/com/google/mediapipe/apps/basic/assets:big_buck_bunny.mp4",
        "//mediapipe/examples/android/src/java/com/google/mediapipe/apps/basic/assets:spiderman.mp4",
    ],
    assets_dir = "",
    manifest = "AndroidManifest.xml",
    resource_files = glob(["res/**"]),
    visibility = ["//visibility:public"],
    deps = [
        "//mediapipe/java/com/google/mediapipe/components:android_camerax_helper",
        "//mediapipe/java/com/google/mediapipe/components:android_components",
        "//mediapipe/java/com/google/mediapipe/framework:android_framework",
        "//mediapipe/java/com/google/mediapipe/glutil",
        "//third_party:androidx_appcompat",
        "//third_party:androidx_constraint_layout",
        "//third_party:opencv",
        "@maven//:androidx_concurrent_concurrent_futures",
        "@maven//:com_google_guava_guava",
    ],
)

# Manifest common across example apps.
exports_files(
    srcs = ["AndroidManifest.xml"],
)

# Native dependencies to perform edge detection in the Hello World example.
cc_binary(
    name = "libmediapipe_jni.so",
    linkshared = 1,
    linkstatic = 1,
    deps = [
        "//mediapipe/graphs/hand_tracking:mobile_calculators",
        "//mediapipe/java/com/google/mediapipe/framework/jni:mediapipe_framework_jni",
    ],
)

# Converts the .so cc_binary into a cc_library, to be consumed in an android_binary.
cc_library(
    name = "mediapipe_jni_lib",
    srcs = [":libmediapipe_jni.so"],
    alwayslink = 1,
)

# Hello World example app.
android_binary(
    name = "helloworld",
    assets = [
        "//mediapipe/graphs/hand_tracking:hand_tracking_mobile_gpu.binarypb",
        "//mediapipe/modules/hand_landmark:handedness.txt",
        "//mediapipe/modules/hand_landmark:hand_landmark.tflite",
        "//mediapipe/framework/formats:landmark_proto",
        "//mediapipe/modules/palm_detection:palm_detection.tflite",
    ],
    assets_dir = "",
    manifest = "AndroidManifest.xml",
    manifest_values = {
        "applicationId": "com.google.mediapipe.apps.basic",
        "appName": "Hello World",
        "mainActivity": ".MainActivity",
        "cameraFacingFront": "True",
        "binaryGraphName": "hand_tracking_mobile_gpu.binarypb",
        "inputVideoStreamName": "input_video",
        "outputVideoStreamName": "output_video",
        "flipFramesVertically": "True",
        "converterNumBuffers": "2",
    },
    multidex = "native",
    deps = [
        #        ":assets_dep",
        ":handcontrol",
        ":mediapipe_jni_lib",
        "//mediapipe/framework/formats:detection_proto",
        "//mediapipe/java/com/google/mediapipe/framework:android_framework",
    ],
)
