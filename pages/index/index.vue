<template>
    <view class="content">
        <view class="">
            <button type="primary" @tap="e_shootFront">拍摄照片</button>
        </view>
        <view class="">
            <button type="primary" @tap="e_shootFace">拍摄视频</button>
        </view>
    </view>
</template>

<script>
    export default {
        data: {
            title: 'Hello'
        },
        methods: {
            e_shootFront() {
                uni.chooseImage({
                    count: 1,
                    sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
                    sourceType: ['album', 'camera'], //从相册选择,拍摄
                    success: function (res) {
                        //#ifdef APP-PLUS
                        plus.nativeUI.alert(JSON.stringify(res));
                        const tempFilePaths = res.tempFilePaths[0];
                        plus.nativeUI.alert(tempFilePaths);
                        uni.saveImageToPhotosAlbum({
                            filePath: tempFilePaths,
                            success: function () {
                                // plus.nativeUI.alert('save success');

                            }
                        });
                        plus.io.resolveLocalFileSystemURL(tempFilePaths, function (entry) {
                            entry.file(function (file) {
                                const fileReader = new plus.io.FileReader();
                                fileReader.readAsDataURL(file, 'utf-8');
                                fileReader.onloadend = function (evt) {
                                    plus.nativeUI.alert(evt.target.fileName);
                                }
								plus.nativeUI.alert('success');
                            });
                        }, function (e) {
                            plus.nativeUI.alert("Resolve file URL failed: " + e.message);
                        });
                        //#endif
                    }
                });
            },
            e_shootFace() {
                uni.chooseVideo({
                    count: 1,
                    sourceType: ['album', 'camera'],
                    success: function (res) {
                        //#ifdef APP-PLUS
                        plus.nativeUI.alert(JSON.stringify(res));
                        const tempFilePath = res.tempFilePath;
                        uni.saveVideoToPhotosAlbum({
                            filePath: tempFilePath,
                            success: function () {
                                plus.nativeUI.alert('save success');

                            }
                        });
                        plus.nativeUI.alert(tempFilePath);
                        const path = tempFilePath;
                        plus.io.resolveLocalFileSystemURL(path, function (entry) {
                            entry.file(function (file) {
                                const fileReader = new plus.io.FileReader();
                                fileReader.readAsDataURL(file);
                                fileReader.onloadend = function (evt) {
                                    plus.nativeUI.alert('onloadend');
                                    plus.nativeUI.alert(evt.target.fileName);
                                }
                            });
                        }, function (e) {
                            plus.nativeUI.alert("Resolve file URL failed: " + JSON.stringify(e));
                        });
                        //#endif
                    }
                });
            },
        },
    }
</script>

<style>
    .content {
        flex: 1;
        justify-content: center;
        align-items: center;
    }

    .title {
        font-size: 36px;
        color: #8f8f94;
    }
</style>
