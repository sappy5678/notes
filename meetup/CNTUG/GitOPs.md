---
title: GitOPs
categories:
  - DevOps
tags:
  - Kubernetes
  - Git
  - CNTUG
date: 2020-08-22T15:36:28.559Z
---

本篇是 CNTUG Meetups 的簡短心得

# 概念
1. 所有環境的變動變動都在 git 發生並記錄在 git
2. 能從 git 自動部屬

因此如果用是普通的 CI/CD ，在沒有人工介入修改行為一切都是用 git tigger 的情況下，應該也可以稱為 gitops

# 實作建議
1. Application repo 跟 environment repo 分開
2. push/pull base -> 推薦使用 pull base
   * push base 跟傳統 CD 差不多，較不推薦使用
   * pull base 推薦使用，自動化 process 放在 k8s 中，K8S 定期拉 git 上的環境設定，並自動更新環境，改 images tag 也會自動更新到 git 上的 env repo 

# 優點
* 進退版方便
* 確保環境一致，不會有人為修改


# 實作
* ArgoCD
* Flux
* Pulumi

## ArgoCD
### 特性
* 原生做不到金絲雀部屬，要使用 argo rollout
* for K8S only
* pull base
* 只觀察 git repo
* dex - 預設搭配作認證
* repo-server - 觀察 github
* 可以部屬到多個 cluster
* 能看到人為修改差異
* Redhat 推薦XD
* history 放在 CRD 裡面，因此 CRD 不能亂動
* 會存資料在 CRD & configMap
* 講者認為 ArgoCD > flux


## Flux
* 原生做不到金絲雀部屬
* pull base
* 追蹤 git + docker images
* semver version or 時間 來判斷哪個 docker images 最新

# 其他
## 其他建議
1. 部屬在哪裡 -> 看資安政策，一般來說 prod 去連 dev 比較不會被刁難， dev 去連 prod 比較會被刁難
2. Docker images 不要用 latest 做命名
3. k8s master 絕對絕對不能開 public internet

## 有趣問題
* 使用 gitops，如果 running state 因為 HPA 把 replicas 變多(或少)時，上版後會不會被改回去？因為 deployment .yaml 裡面的 replicas 不會被 HPA 改到 -> 不過講者說 gitops 有能力做到，這問題可被修正  

* 之前我用試著用程式回寫 git repo，結果造成無窮迴圈。 (git 觸發 jenkins 觸發 kubernetes 寫回 git 再觸發 jenkins 觸發 kubernetes ) -> 講者說的回覆是開源幫你做完惹

* got it, 所以如果用經典的pipline來講 沒有人工介入修改行為一切都是用git tigger 所以也可以講 gitops XD -> 講者說的回覆是說這是正確的