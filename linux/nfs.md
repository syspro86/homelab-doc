# nfs

insecure 기본 포트외 접근을 허용한다.

root_squash : client 의 root로 접근시, 서버의 root가 아닌 임의 계정으로 매핑한다.

no_root_squash : client 의 root 접근 시 서버의 root로 매핑한다.

all_squash : client 의 모든 접근 계정에 대해, 서버의 임의 계정으로 매핑한다.

anonuid, anongid : root_squash, all_squash 일때 매핑되는 uid, gid

subtree_check : 파일시스템의 root가 export 되지 않고, 하위 폴더만 export 되어있을 때, client에서 접근하는 경로가 제대로 export 된 하위 경로인지 체크하는 옵션. no_subtree_check 인경우, export 되지 않은 상위 폴더까지 접근이 가능해지는 문제가 있지만, subtree_check 의  부작용이 더 커서 기본옵션이었다가, no_subtree_check가 기본옵션이 됨. (nfs stale handle)

