<h1 align="center"> Empower Chain Güncelleme</h1>

````
cd || return
git clone https://github.com/EmpowerPlastic/empowerchain
cd empowerchain/chain || return
git checkout v1.0.0-rc2
make install
empowerd version # 1.0.0-rc2
```
# nodumuzu başlatıyoruz
```
sudo systemctl restart empowerd && sudo journalctl -u empowerd -f --no-hostname -o cat
```