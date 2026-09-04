为 restic backup 增加按文件修改时间（mtime）过滤备份文件的能力。

当前 restic 支持按大小排除文件（--exclude-larger-than），但没有按修改时间排除文件的选项。请为 backup 命令新增两个全局过滤选项：--exclude-older-than 排除修改时间早于当前时间减去 duration 的文件，即只备份比该时间更新的文件；--exclude-newer-than 排除修改时间晚于当前时间减去 duration 的文件，即只备份比该时间更旧的文件。duration 采用与 forget --keep-within 相同的时长语法，例如 24h、30d、1y。

两个选项可单独使用，也可同时使用，同时使用时排除命中并集，仅保留 mtime 落在两者之间的文件。只过滤文件，不阻止目录继续向下遍历，与 --exclude-larger-than 一致。与 --exclude、--include、--files-from 等现有过滤器叠加生效。duration 非法或为负值时报错并退出非 0。未传这两个选项时，备份行为与之前完全一致。
