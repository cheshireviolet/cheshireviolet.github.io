+++
date = '2025-03-03T20:52:02-03:00'
draft = true
title = 'Project Mono Parte 2 - Mono'
tags = ['Reverse Engineering', 'Project Mono']
+++

Bom dia amigos! O último post foi meio rushado, sem muita informação meio por cima, decidi fazer a parte dois um pouquinho mais detalhada, começando com alguns pontos anteriores.

## dnSpy - Descobrindo a estrutura das classes do jogo

[Github](https://github.com/dnSpy/dnSpy)

É um debugger e decompiler maravilhoso para .NET e jogos em Unity, que é o caso do Click Click Dig. Tudo que a gente precisa está no arquivo `Assembly-CSharp.dll`, que você normalmente encontra no diretório Managed, no diretório do próprio jogo.

![dnSpy com o Assembly-CSharp.dll](/static/sixth/dnspy.png).

Vão ser listadas todas as classes, até mesmo classes que não são usadas, porém são muitas. O que eu precisava era uma função que cuidasse dos inputs OU que ativasse as skills.

No nosso caso, existem 3 funções muito interessantes, todas dentro da classe `heroSkill`: `UseSkill()`, `TriggerSkill()` e `HandleSkillClicked()`.

```C#
public void UseSkill()
{
    this.HandleSkillClicked();
}
```

```C#
private void TriggerSkill()
{
    switch (this.mHeroSkillDef.defId)
    {
    case 0:
        Singleton<ClickGame>.Instance.EnableAutoClick(true, (int)this.mHeroSkillData.currentAmount);
        return;
    case 1:
        Singleton<ClickGame>.Instance.player.ModifyMoneyMultiplierSkills(this.mHeroSkillData.currentAmount / 100.0);
        return;
    case 2:
        Singleton<ClickGame>.Instance.player.ModifyDamageMultiplierSkillsClickDamage(this.mHeroSkillData.currentAmount / 100.0);
        return;
    case 3:
        Singleton<ClickGame>.Instance.player.ModifyDamageMultiplierSkillsAllDiggers(this.mHeroSkillData.currentAmount / 100.0);
        return;
    case 4:
        Singleton<ClickGame>.Instance.player.ModifyStat(PlayerStatType.DirtLayerCoinValuePercentDropPerTapSkills, this.mHeroSkillData.currentAmount);
        Singleton<ClickGame>.Instance.EnableMoneyPerClick(true);
        return;
    case 5:
        Singleton<ClickGame>.Instance.CreateItemDrops(ItemDropType.KEY, 1.0, -1);
        if (!Singleton<PlayerDataManager>.Instance.FlagIsSet(FlagType.SHOW_TREASURE_CHEST_UI))
        {
            Singleton<ClickGame>.Instance.EnableTreasureChestSystem();
        }
        return;
    default:
        return;
    }
}
```

```C#
private void HandleSkillClicked()
{
    if (!this.mAllowClicks)
    {
        return;
    }
    SoundEffect.Play("use_hero_skill", 1f, null, 0f);
    this.TriggerSkill();
    Singleton<PlayerDataManager>.Instance.ModifyStat(GameStatType.PROSPECTOR_SKILLS_USED, 1.0);
    this.mHeroSkillData.timeLastActivated = SafeDateTime.UtcNow;
    if (this.mHeroSkillDef.isInstantUse)
    {
        this.EnterCooldown(true);
        return;
    }
    this.mHeroSkillData.isActive = true;
    this.mSkillActiveTimeRemainingS = (float)this.mHeroSkillDef.durationSeconds;
    this.activeTimeRemainingLabel.text = Helpers.FormatTimeMinutesAndSeconds(Mathf.Round(this.mSkillActiveTimeRemainingS), false);
    this.EnterCooldown(true);
    this.cooldownLabel.gameObject.SetActive(false);
    this.activeTimeRemainingLabel.gameObject.SetActive(true);
}
```
