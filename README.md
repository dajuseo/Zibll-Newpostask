# 子比考试系统 (Zibll Theme NewAsk Admin)

为子比主题添加考试功能。

## 前言

- 本插件为二开，原作者为 Le Vent，原作者 URI: [https://acg.la](https://acg.la)
- 本插件功能开源免费使用并更新，但是请保留底部作者版权，自觉遵守开源协议。如果觉得好用还请点个 star！
- 注意，本插件还在开发，后台管理功能部分正在完善。  
- 支持二开移植，但是请保留作者版权，谢谢！  
- 本插件会持续保持更新。
- 赞助：[https://semfollow.com](https://semfollow.com)

## 功能

- [X] 基础功能保证
- [X] 支持创建题目
- [x] 前端答题页面和随机出题最终评分
- [x] 前端查看考试历史
- [x] 无后台版纯数据库投入使用
- [x] 后台加题，答题考试管理插件
- [x] 在线获取更新
- [x] 消耗积分或者余额答题
- [ ] 可以修改总分
- [ ] 可以修改及格线
- [x] 可以修改前台提示
- [X] 答题成功后选择是否给予会员
- [x] 自定义奖励
- [X] 一键导入题目
- [ ] 注册答题功能
- [ ] 设置不同题目难度类目，DIY 考试类目
- [ ] DIY 投稿权限，设置不同分数获得不同的权限，并支持进阶题类目重考。

## 投稿考试安装教程

1. 上传 `newpostask.php` 到 `zibll/pages/` (newpostask.php 是在压缩包内哦)。
2. 将 `plugins` 里的 `newask` 文件夹上传到 wp 插件目录并激活，此步骤会自动创建数据表并插入默认题目。
3. WordPress 后台/页面/添加页面 新建页面，页面模板选择“投稿考试”，固定链接设置为 `newask`。
4. 在 `zibll/pages/newpost.php` 第 12 行添加以下代码：

    ```php
    // 考试答题
    $uid = get_current_user_id();
    $sql_ck = "SELECT * FROM `wp_fl_meta` WHERE `meta_id` = '$uid' AND `meta_key` = 'newask'";
    $row_ck = $wpdb->get_row($sql_ck, ARRAY_A);
    if($row_ck['meta_var'] != '1'){
        header('Location:/newask');
        exit;
    }
5. 若是数据表没有创建可以手动运行以下 SQL 语句创建：
    ```Mysql
    CREATE TABLE `fl_ask_tm` (
      `ID` int(11) NOT NULL,
      `type` varchar(255) NOT NULL,
      `name` longtext NOT NULL,
      `all_answer` longtext DEFAULT NULL,
      `answer` varchar(255) DEFAULT NULL,
      `tips` longtext DEFAULT NULL,
      `score` int(255) NOT NULL
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_520_ci;
    
    INSERT INTO `fl_ask_tm` (`ID`, `type`, `name`, `all_answer`, `answer`, `tips`, `score`) VALUES
    (1, '标题规范题', '以下图包投稿类标题哪个是正确的？', '【Fantia/图包/无修正】JIMA大佬22.10-23.01作品合集（36P/560M）|【Fantia/图包/无修正】JIMA大佬22.10-23.01作品合集（560M） |【Fantia/图包/无修正】JIMA大佬22.10-23.01作品合集（36P）|【Fantia/图包/无修正】JIMA大佬22.10-23.01作品合集', 'A', '规范中表明了投稿标题格式应为：【分类】资源名称【数量/大小】，非图片/视频类投稿因标注大小，求物类因标注分类。', 5),
    (2, '内容规范题', '以下投稿所设置的解压密码是否存在违规？如果有，请选出违规密码。', 'boluoyyds|acg.la|flcy.us|pornhub.com', 'D', '投稿规范中说明了解压密码推荐使用 acg.la 或本站网址，禁止直接使用其他网站的网址为解压密码，可以自行更改解压码为 acg.la 后压缩上传 （如果你是搬运，最好经过原主同意）', 10),
    (3, '内容规范题', '用以下哪种网盘进行分享的文件是不符合本站要求的？', '百度网盘|谷歌网盘|磁力链接|城通网盘', 'D', '本提请参考投稿内容规范，规范内明确注明了网盘链接分享规范。', 5),
    (4, '内容规范题', '以下哪种情况下，打码合格？', '可以看到乳头形状的投稿|色块、图片遮挡|用国家领导人的照片等涉及政治敏感信息的图片进行打码的投稿|半透明马赛克打码的投稿', 'B', '根据预览规范第四条规定：打码 不打码的一次警告二次封禁 敏感的三点一点都不能露，露出部分 X 头、鲍鱼都是违规打码！ 建议使用图片、圣光遮挡，一般的马赛克容易被打回。', 10),
    (5, '基本常识题', 'RJ 号/DMM 编号实质是什么？', '并无实质作用|DLsite/DMM 发售作品时给予作品的唯一编号（也位于链接处），可以作为方便审核确认站内是否撞车的依据|等同于商业作的发售日期|卡小白投稿的没用玩意', 'B', '请自行 GOOGLE 了解。', 5),
    (6, '内容规范题', '以下哪种 Q 群的宣传可以附在本站投稿正文中?', '某某官方资源发布群|UP 自己加入的某个游戏的交流群|汉化组的工作交流群、工作群（带广告性质的收费群除外）|UP 自己建立的资源内部发布群', 'C', '本站禁止发布任何带有收费或广告性质的内容，群组推广。', 10),
    (7, '内容规范题', '以下哪种投稿的正文简介符合本站的最低要求', '与资源内容相关的极为简短的介绍，如“纯爱、调教、NTR”等|没有提到任何与资源本身内容相关的话题，只是写了一些 UP 自己近期身边的事|整个正文只有“。。。。。。。。。。。”或者“游戏还行，能玩”|整个正文中只有“解压密码为：acg.la”', 'A', '遵从心意。', 5),
    (8, '查重规范题', '以下哪些情况下，投稿不视作撞车/可以正常通过？(送分题)', '同一位 UP 连续投稿的同一个本子的不同汉化组汉化的版本|站内已有该资源，另一位 UP 投稿的该资源的度盘分流档|由不同 UP 投稿的同一个本子的不同汉化组汉化的版本或原资源已经被爆，由另一位 UP 投的符合站内相关规定的补档投稿|复制粘贴，修改标题直接发布', 'C', '送分题。', 5),
    (9, '内容规范题', '以下哪些选项是本站不接受投稿的资源类型？', '岛国大片/AV/未成年|全年龄游戏|全年龄图包|全年龄 COS', 'A', '本站禁止发布任何岛国，不知名女优，性爱视频，尤其是未成年，一旦发布，删除并永久封号，绝不姑息。', 5),
    (10, '分类规范题', '请选出以下给出选项中分类存在错误的选项', '动画区：【脸肿字幕组】THE ANIMATION 【2V/150M】|图画区：【图包】gawd 大佬三月合集【100P/550M】|图画区：【动画】vnk 一月 MMD 新作【100P/550M】|GAME 区：【3D】老滚 5 自整理 MOD 大合集【80G】', 'C', '分类规范在投稿规范中已明确标明。', 5),
    (11, '隐藏内容题', '以下哪个隐藏方式禁止使用？', '评论可见|积分阅读|登录可见|不隐藏', 'A', '请查阅投稿规范。', 5),
    (12, '积分设置题', '以下哪些资源积分设置不合理？', '【动画】vnk 大佬一月合集【5V/200M】：6 积分|【图包】vnk 大佬一月合集【50P/200M】：1 积分|【RPG】家出少女 1.2.10 汉化版【200M】：20 积分|【动画】vnk 大佬所有作品合集【50V/20G】：20 积分', 'C', '单个资源（包括单个资源和作者某月的合集）最高限制设置 6 积分，超过不通过，合集资源（资源作者发布的所有作品合集）最高限制设置 20 积分，超过不通过！', 10),
    (13, '预览规范题', '关于预览图，以下说法正确的是？', '投稿时可以只设置封面不插入预览图至正文|投稿时可以不上传预览图或封面|预览图可以不打码直接上传|投稿时应正确设置封面并将预览图插入至正文，否则会不通过。', 'D', '请查阅投稿规范。', 10),
    (14, '标题规范题', '汉化者名字未知时，汉化者一栏应当如何填写？', '直接不写汉化者也可以|匿名汉化者|中国翻译|汉化者不明 或 未知汉化', 'A', '未知汉化资源可以不标明汉化作品来源。', 5),
    (15, '内容规范题', '关于投稿以下哪种说法是正确的？', '发布自购资源时需要带上自购证明至正文|搬运他人作品时可以不标注作品来源|发布资源合购信息，邀请大家一起合购|发布资源时只使用诚通网盘作为资源主链接。', 'A', '请查阅投稿规范。', 5),
    (16, '内容规范题', '关于资源链接，以下哪些链接是正确的？', 'xxxxxx.com/：7 天有效期|xxxxxx.com/：30 天有效期|xxxxxx.com/：1 天有效期|xxxxxx.com/：永久有效', 'D', '本站明令禁止使用短效链接。', 5),
    (17, '压缩规范题', '用以下哪种压缩软件压缩的文件不符合要求的', 'WINRAR|Bandzip|7-ZIP|快压', 'D', '禁止使用任何国产恶意收费压缩软件。', 5);
    
    ALTER TABLE `fl_ask_tm`
      ADD PRIMARY KEY (`ID`);
    
    ALTER TABLE `fl_ask_tm`
      MODIFY `ID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=18;
    
    COMMIT;

    ```
6. 添加题目需要注意的是答案选项需要用 | 分开，最多四个选项。例如 答案1|答案2|答案3|答案4

## LICENSE
[GPL-3.0 license](https://github.com/znc15/Zibll-Newpostask?tab=GPL-3.0-1-ov-file#readme)

## 截图
![](https://i1.mcobj.com/imgb/u15prb/20240711_668f83f6a3c91.png)
![](https://i1.mcobj.com/imgb/u15prb/20240711_668f83f7414be.png)
![](https://i1.mcobj.com/imgb/u15prb/20240711_668f83f7c71c5.png)

## 问题反馈
[Telegram](https://t.me/Count_API)
 
[Issue](https://github.com/znc15/Zibll-Newpostask/issues)
