(defn- micropub [{:keys [cfg headers json-params params] :as req}] 
  (try
    ((let [{:keys [mp-syndicated-to] :as params-kw} (keywordize-keys (or json-params params))
           {id :id token :token} (token-header->id req)
           provider (trim (:host (uri id)))
           {:keys [isn permafrag] :as post-data} (dispatch-post {:cfg cfg :m (assoc params-kw :provider provider)})
           in (cond (nil? (headers "authorization")) :400 (not (authcn? {:cfg cfg :id id :isn isn})) :401 (empty? post-data) :400 :else :201)]
       (condp = in
         :400 (->400 "bad request - please check your request is spec compliant")
         :401 (->401 "unauthorized - credentials or token not valid")
         :201 (let [loc-hdr (str site-root "/" permafrag)]
                (its/create pr-fs (str "/" permafrag ".edn") post-data)
                (sse-send (json/write-str {:name "isn-signal" :data post-data}))
                (->201 loc-hdr "post has been created")))))
    (catch Exception err
      (println "Error in micropub" err)
      (->400 "Error caught"))
    (finally
      (println "Error"))))